### WebSocket

#### 相关协议

- 互联网协议

- - TCP/IP协议

  - - 定义了电子设备如何连入因特网，以及数据在它们之间传输的标准
    - 传输数据（协议）类型：Email、www、FTP

  - HTTP协议

  - - 浏览器和万维网服务器之间相互相互通信的规则



- HTTP协议特点

- - 功能很强大
  - 采用请求、响应模式，单向通信
  - 短链接，响应完成连接就断开



- WebSocket 
  - 是HTML5开始提供的一种在单个TCP连接中实现双向通讯的协议
  - 允许服务器主动向客户端发送消息，服务端与客户端只需要完成一次握手就可以创建持久性的连接，并且可以互相发送数据
  - 支持WebSocket的主流浏览器如下：
    - Chrom、Firefox、IE >= 10、Sarafi >= 6、Android >= 4.4、iOS >= 8



#### nodejs与WebSocket结合

- 建立socket应用？

- - 服务器必须支持webSocket

  - Nodejs的简介

  - - Ryan DAhl基于Goggle v8引擎创建的一套用来编写高性能网络服务器的ECMAScript工具包

  - Nodejs:用js去写服务器应用



- 什么是socket.io ?
  - Socket.io是node.js的一个模块，它是通过WebSocket进行通信的一种简单方式。
  - Socket.IO提供服务器和客户端双方的组件，所以只需一个模块就可以给应用程序加入对WebSocket的支持 （如果想实现浏览器与web服务器之间的通信node.js+socket.io是很好的选择）
  - Socket.IO也解决了各浏览器的支持问题（不是所有浏览器都支持WebSocket）并让实时通信可以跨几乎所有常用的浏览器实现。



- socket.io示例程序

  - 确保项目已经运行在本地服务器http://127.0.0.1上
  - 确保已经安装node，进入（cd E:\myproject）到项目目录中，创建socket文件夹存放服务端文件，创建配置文件package.json来声明依赖模块

  ``` json
  {
      "name":"socket.io",
      "version":"0.0.1",
      "dependencies":{
          "socket.io":"0.8.7"
      }
  }
  ```

  - 在socket文件夹中创建服务文件service.js监听连接

  ``` JS
  //服务器端监听socket
  var http = require('http');
  var fs = require('fs');
  var io = require('socket.io');   //引入模块
  
  var documentRoot = 'http://127.0.0.1';
  
  var httpServer = http.createServer(function (req, res) {
    var url = req.url;
    var file = documentRoot + url;
    console.log(file);
    fs.readFile(file, function (err, data) {
      if (err) {
        res.writeHeader(404, {
          'content-type': 'text/html;charset="utf-8"'
        });
        res.write('<h1>404</h1><p>NO Found</p>');
        res.end();
      } else {
        res.writeHeader(200, {
          'content-type': 'text/html;charset="utf-8"'
        });
        res.write(data);
        res.end();
      }
    });
  }).listen(3000);
  
  console.log("连接建立完毕");
  var socket = io.listen(httpServer);
  socket.sockets.on('connection', function (socket) {
    //socket  每个连接都有独立的socket对象
    console.log('有人通过socket连接进来了');
    socket.emit('hello', '欢迎');         //emit('事件发送名称','发送的数据')
    
    //socket.broadcast.emit('a');  //向除了该触发事件以外的所有客户端发送
    socket.broadcast.emit('a', '有新人进来了');
    
    //监听客户端，实现一个用户拖动方块，所有用户均可以看见的效果
    socket.on('move', function (data) {
      socket.broadcast.emit('move2', data);
    });
  });
  ```

  - 在项目文件夹中创建index.html，引入socket.io.js，创建index.js文件与服务器建立连接（注意运行service.js后生成的socket.io.js为服务器根目录的/socket.io/socket.io.js）

  ```
  <script src="127.0.0.1:3000/socket.io/socket.io.js"></script>
  <script src = "index.js"></script>
  ```

  ```JS
  window.onload = function () {
    //客户端发送socket
    var oBtn = document.getElementById('btn');
    var oDiv = document.getElementById('div1');
    var socket = null;
  
    oBtn.onclick = function () {
      socket = io.connect('http://127.0.0.1:3000');  //与服务器端建立连接
      socket.on('hello', function (data) {
        alert(data);
        this.emit('hellotoo', '欢迎欢迎');
      });
      
       socket.on('a', function(data) {
         alert(data);
       });
  
      socket.on('move2', function (data) {
        oDiv.style.left = data.left + 'px';
        oDiv.style.top = data.top + 'px';
      });
    }
  
    //同步拖拽事件（就像游戏场景，所有客户端可以看到相同的场景）
    oDiv.onmousedown = function (ev) {
      var ev = ev || event;
      var disX = ev.clientX - this.offsetLeft;
      var disY = ev.clientY - this.offsetTop;
  
      if (oDiv.setCapture) {
        oDiv.setCapture();
      }
  
      document.onmousemove = function (ev) {
        var ev = ev || event;
        oDiv.style.left = ev.clientX - disX + 'px';
        oDiv.style.top = ev.clientY - disY + 'px';
  
        socket.emit('move', {
          left: oDiv.offsetLeft,   //可以直接传对象
          top: oDiv.offsetTop
        });
      }
  
      document.onmouseup = function () {
        document.onmousemove = null;
        //releaseCapture : 释放全局捕获
        if (oDiv.releaseCapture) {
          oDiv.releaseCapture();
        }
      }
      return false;
    }
  }
  ```

  - 在项目文件夹下，运行命令` npm install`完成socket.io的安装，执行命令之后会在该目录下创建模块node_modules
  - 进入到socket文件夹下使用命令` node service.js`启动服务器
  - 在浏览器中输入http://127.0.0.1可以访问到页面，点击页面中的button，弹出“欢迎”提示框，在终端输出“'有人通过socket连接进来了”，当同时打开两个页面时，拖动一个页面中的方块div1另一个页面中方块也在移动。



#### applicationCache

- 离线存储的好处？

- - 没网的时候，可以正常访问
  - 快速响应页面，不必用多个HTTP占用资源带宽
  - 缓存的可以是任何文件



- 搭建离线应用程序

- - ① 服务器设置头信息 :（在apche服务器下的httpd.conf文件中添加下面的代码）

  - -  AddType text/cache-manifest .manifest

  - ② html标签加 : <html manifest = "cache.manifest">    //文件名最好是是英文

  - - manifest=“xxxxx.manifest”

  - ③ 写manifest文件 :  离线的清单列表（新建文件）

  - 先写 :  CACHE MANIFEST

  - FALLBACK : 第一个网络地址没获取到，就走第二个缓存的（空格隔开）

  - NETWORK ：无论缓存中存在与否，均从网络获取

  - CACHE MANIFEST FallBack style.css style2.css    

```JS
CACHE MANIFEST
FallBack
style.css style2.css   
```



#### Web  Workers

- 什么是worker?

- - JS的单线程（放入UI队列的个数，利用定时器解决）
  - 可以让web应用程序具备后台处理能力，对多线程的支持非常好。new Worker('test.js');



- Worker API

- - new Worker(‘后台处理的JS地址’)
  - 利用 postMessage 传输数据在test.js中通过

```JS
self.onmessage = function(ev){   //监听数据传输
    ev.data  //获取数据
    
    self.postMessage('回传计算后的数据');
}
```

- - importScripts (‘导入其他JS文件’)



- Worker运行环境

- - navgator  :  appName、appVersion、userAgent、platform
  - location  :   所有属性都是只读的
  - self  :  指向全局 worker 对象
  - 所有的ECMA对象，Object、Array、Date等
  - XMLHttpRequest构造器
  - setTimeout和setInterval方法
  - close()方法，立刻停止worker运行  w1.close();
  - importScripts方法（在worker中使用外部JS文件中的方法，需要先引入）



#### HTML5其他功能

**内容编辑**

- contenteditable = "true“



**语音输入**

` <input type="text" x-webkit-speech />     //在输入框后面多一个语音输入按钮，chrome支持`



**桌面提醒（基于桌面的，缩小浏览器依然可见）**

```html
<script>
    (function () {
      if (window.Notification) {
        var btn = document.getElementById("btn");
        var txt = document.getElementById("div1");
        btn.onclick = function () {
          if (Notification.permission == "granted") {
            popNotice();
          } else if (Notification.permission != "denied") {
            Notification.requestPermission().then(function (permission) {
              popNotice()
            })
          }
        };

        function popNotice() {
          if (Notification.permission == "granted") {
            var notification = new Notification("你好:", {
              body: "请问今晚有空吗",
              icon: "./imgs/v2-a47051e92cf74930bedd7469978e6c08_hd.png"
            });
            notification.onclick = function () {
              txt.innerHTML = new Date().toTimeString().split(" ")[0] + "收到信息";
              notification.close();
            }
          }
        }
      } else {
        console.log("浏览器不支持Notification");
      }
    })();
</script>
```

