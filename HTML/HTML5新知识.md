### HTML5新标签



**字符设定**

```html
<meta http-equiv="charset" content="utf-8"> <!--HTML与XHTML中建议这样去写-->
<meta charset="utf-8"> <!--HTML5的标签中建议这样去写-->
```



**常用的新标签**

- header：定义了文档的头部区域

- nav：定义导航链接的部分
- footer：定义 section 或 document（文档） 的页脚
- article：定义页面独立的内容区域（比如论坛的帖子）
- section：定义文档中的节（section、区段）划分区域，没有特别含义
- aside：定义页面的侧边栏内容
- datalist：标签(id 属性)结合input （list属性）实现option列表关键字关联和列表选择
- fieldset 元素可将表单内的相关元素分组，和legend一起搭配使用<filedset><legend>嵌在框内的标题<legend><filedset>
- details：标签内嵌入summary标签（放缩略信息）使用，用于切换详情展示与否
- dialog：定义一段对话，里面放dt、dd标签（open属性设置是否默认展开）
- adress（文章或作者信息）、mark（标记）、keygen（添加公钥）、progress（进度条）



兼容性写法（添加js然后将元素设置为块状元素dispaly:block ）：

```html
<script type="text/javascript">
(function(){
 vare="abbr,article,aside,audio,canvas,datalist,details,figure,
 footer,header,hgroup,mark,menu,meter,nav,output,progress,section,time,video".split(','),
   i=e.length;
   while(i--){
     document.createElement(e[i]);  //兼容不支持新标签的浏览器
   }
}());
</script>
```





**常用的新属性**

—— 保存到草稿箱时，可以给input添加formnovalidate（去掉表单验证）

| **属性**         | **用法**                                                     | **含义**                                                     |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **placeholder**  | <input type="text" placeholder="请输入用户名">               | 占位符提供可描述输入字段预期值的提示信息                     |
| **autofocus**    | <input type="text" autofocus>                                | 规定当页面加载时 input 元素应该自动获得焦点                  |
| **multiple**     | <input type="file" multiple>                                 | 多文件上传                                                   |
| **autocomplete** | <input type="text" autocomplete="off">                       | 规定表单是否应该启用自动完成功能（表单包含提交按钮；input有设置name属性） |
| **required**     | <input type="text" required>                                 | 必填项（浏览器兼容问题）                                     |
| **accesskey**    | <input type="text" accesskey="s">                            | 规定激活（使元素获得焦点）z在这设置获取焦点的快捷键为alt+s   |
| **pattern**      | <input type="text" pattern="\d{1,5}" />                      | 正则验证，输入1到5个数字，但是有安全隐患可破解               |
| **formaction**   | <input type="submit" value="保存到草稿" formaction = “http://www.baidu.com” /> | 定义除了form里面action之外的另一个提交地址                   |



**新增的type属性值**

| **类型**     | **含义**                               |
| ------------ | -------------------------------------- |
| **email**    | 输入邮箱格式，提交时验证提示           |
| **tel**      | 输入手机号码格式                       |
| **url**      | 输入url格式                            |
| **number**   | 输入数字格式                           |
| **search**   | 搜索框（体现语义化）多一个清空文本按钮 |
| **range**    | 自由拖动滑块，可以设置step、min、max   |
| **time**     | 可输入时间控制器（小时和分钟）         |
| **date**     | 日期选择器                             |
| **datetime** | 日期和时间控制器                       |
| **month**    | 选择年月                               |
| week、color  | 选择周和年、选择颜色                   |



**HTML5表单验证反馈**

- validity对象，通过下面的valid可以查看验证是否通过，如果八种验证都通过（都返回false）则valid返回true，一种验证失败返回false

```
this.validity.valueMissing  :  输入值为空时返回true(表单添加了reqiured属性) 

this.validity.typeMismatch :  控件值与预期类型不匹配返回true 

this.validity.patternMismatch  :  输入值不满足pattern正则返回true 

this.validity.tooLong  :  超过maxLength最大限制 

this.validity.rangeUnderflow : 验证的range最小值(type="range") 

this.validity.rangeOverflow：验证的range最大值 

this.validity.stepMismatch: 验证range 的当前值 是否符合min、max及step的规则 

this.validity.customError 不符合自定义验证     »setCustomValidity(); 自定义验证 
```



```js
oText.addEventListener("invalid",fn1,false);  //监听  
function fn(){     
    conosle.log(this.validity)  //验证反馈存在该对象中     
    console.log(this.validity.customError);   //自定义验证的反馈    
    ev.preventDefault();   //阻止默认事件 
} 
oText.oninput = function(){   //使用自定义验证     
    if(this.vlue == '验证'){         
        this.setCustomValidity("验证反馈结果");     
    }else{         
        this.setCustomValidity("");  //一定要清空否则还会有提示     
    } 
}  
```



**多媒体**

**多媒体embed（标签定义嵌入的内容）**

- embed可以用来插入各种多媒体，格式可以是 Midi、Wav、AIFF、AU、MP3等等。src为音频或视频文件的路径，可以是相对路径或绝对路径

```html
<embed src="http://player.youku.com/player.php/sid/XMTI4MzM2MDIwOA==/v.swf" allowFullScreen="true" quality="high" width="480" height="400"  align="middle" allowScriptAccess="always"  type="application/x-shockwave-flash"></embed> 
```



**多媒体audio（HTML5通过该标签解决音频播放）**

```html
<audio controls>
    <source src="./music.mp3">
    <source src="./music.wav">
    <source src="./music.ogg">
    Your browser does not support the audio tag.
</audio>
```

- audio的附加属性：autoplay 自动播放、controls 规定浏览器应该为音频提供播放控件、loop循环播放
- 注： IE 8 或更早版本的 IE 浏览器不支持 <audio> 标签

| 音频格式 | MINE-type  | Internet Explorer | Chrome | Firefox | Safari | Opera |
| -------- | ---------- | ----------------- | ------ | ------- | ------ | ----- |
| MP3      | audio/mpeg |                   |        |         |        |       |
| Ogg      | audio/ogg  | NO                |        |         | NO     |       |
| Wav      | audio/wav  | NO                |        |         |        |       |



**多媒体video（解决视频播放问题）**

```html
<video controls>
  <source src="movie.mp4" type="video/mp4">
  <source src="movie.ogg" type="video/ogg">
  Your browser does not support the video tag.
</video>
```



- audio的附加属性：autoplay 自动播放、controls 规定浏览器应该为音频提供播放控件、loop循环播放、width 播放窗口的宽度、height 播放窗口的高度
- 注： IE 8 或更早版本的 IE 浏览器不支持 <vedeo> 标签

| 视频格式 | MINE-type  | Internet Explorer | Chrome | Firefox                                | Safari | Opera      |
| -------- | ---------- | ----------------- | ------ | -------------------------------------- | ------ | ---------- |
| MP4      | video/mp4  |                   |        | Firefox 21+、 Linux 系统: Firefox 30 + |        | Opera 25 + |
| WebM     | video/webm | NO                |        |                                        | NO     |            |
| Ogg      | video/ogg  | NO                |        |                                        | NO     |            |



**选择器**

> 新的选择器

——查看支持情况 http://www.caniuse.com/#index

- querySelector
- querySelectorAll
- getElementsByClassName



**获取class列表属性**

- classList

- - length :  class的长度
  - add()  :  添加class方法
  - remove()  :  删除class方法
  - toggle() :  切换class方法 



**JSON的新方法**

> parse() : 把字符串转成json

- 字符串中的属性要严格的加上引号



> stringify() : 把json转化成字符串

- 会自动的把双引号加上

```js
JSON.stringify(["上海","北京"]) 
```



> 新方法与eval的区别

- eval：可以解析任何字符串编程JS（不推荐使用）
- parse:只能解析JSON形式的字符串变成JS（安全性更高）

```js
//eval()使用

 var str = 'function test(){alert("sth")}' eval(str);   //将字符串转成JS

 test();   //调用函数 
```



> 新方法的应用

- 深度克隆新对象（对象直接赋值会存在对象引用的问题）

- - 解决方法一： var(for attr in obj) {//这里进行属性赋值}    —— 浅拷贝，对于属性也是对象则不成立

```javascript
//新方法 

var a = {     name : 'hello' };  

var str = JSON.stringfy(a); 

var b = JSON.parse(str); 
```



> 其他浏览器做到兼容—— IE低版本

- http://www.json.org/  找到javascript下面去下载json2.js，引入页面



**data自定义数据**

> dataset

- Data数据在jquery mobile中有着重要作用

data-name :  ele.dataset.name获取 

data-name-first  :  ele.dataset.nameFirst   

```html
<ul>   <li data-index = 0></li>  </ul> 
```



**延迟加载JS**

- JS的加载会影响后面的内容加载

- 很多浏览器都采用了并行加载JS，但还是会影响其他内容

- - Html5的 defer 和 async 	

  - - defer : 延迟加载，会按顺序执行，在onload执行前被触发
    - async : 异步加载，加载完就触发，有顺序问题（不确定哪个先加载完成）

  - Labjs库 —— 异步加载库https://github.com/getify/LABjs

```html
<script src="a.js" defer="defer"></script>   //给外联加
```



**历史管理**

> onhashchange  

- onhashchange 改变hash值来管理

- -  当hash值改变的时候就会触发该事件

```javascript
oEle.onclick = function(){
    for(var i=0; i<arr.length){
        wondow.location.hash = i;
    }
}

window.onhashchange = function(){
    var num = arr[window.location.hash.substring(1)];   //将#去除
}
```



> history  

- 服务器下运行

- - pushState :  三个参数 ：数据  标题(都没实现)  地址(可选  加地址后刷新不存在)
  - popstate事件 :  读取数据   event.state
  - 注意：网址是虚假的，需在服务器指定对应页面，不然刷新找不到页面

```javascript
history.pushState(arr[i],'',i);   //存储 标题空来代替

window.popstate = function(ev){
    var num = ev.state;   //读取存储的历史内容
}
```



**拖放事件**

**draggable** 

- draggable ：设置为true，元素就可以拖拽了（FireFox下有问题）

- 拖拽元素事件 :  事件对象为被拖拽元素

- - dragstart ,  拖拽前触发 （不是按下，要拖拽那一刻）
  - drag ,拖拽前、拖拽结束之间，连续触发（鼠标不动只要没有抬起就仍然执行）
  - dragend  , 拖拽结束触发

- 目标元素事件 :  事元素件对象为目标

- - dragenter ,  进入目标元素触发，相当于mouseover
  - dragover  ,进入目标、离开目标之间，连续触发
  - dragleave ,  离开目标元素触发，相当于mouseout
  - drop  ,  在目标元素上释放鼠标触发；要触发该事件则不需在dragover中阻止默认事件

oEle.ondragstart = function(){} 



事件执行顺序

- 事件的执行顺序 ：drop不触发的时候

- - dragstart  >  drag >  dragenter >  dragover >  dragleave > dragend 



- 事件的执行顺序 ：drop触发的时候(dragover的时候阻止默认事件)

- - dragstart  >  drag >  dragenter >  dragover >  drop > dragend



- 不能释放的光标（禁止符）和能释放的光标（箭头符）不一样



> 火狐下的问题

- 必须设置dataTransfer对象才可以拖拽除图片外的其他标签，否则加了draggable = “true"也不能拖拽

```js
oEle.ondragstart = function(ev){
    var ev = ev||window.evnt;
    ev.dataTransfer.setData('name','yanmei')  //解决火狐下的拖拽问题
}
```



- dataTransfer对象

- - setData() : 设置数据 key和value(必须是字符串)	
  - getData() : 获取数据，根据key值，获取对应的value



> dataTransfer对象

- effectAllowed 

- - effectAllowed : 设置光标样式(none, copy, copyLink, copyMove, link, linkMove, move, all 和 uninitialized)	

```js
 ev.dataTransfer.effectAllowed = 'copy'; 
```



- setDragImage 

- - 三个参数：指定的元素，坐标X，坐标Y（鼠标在拖拽元素中的位置）

```js
oEle.ondragstart = function(ev){
     //设置拖拽的元素
     ev.dataTransfer.setDragstart(oDiv,0,0)
 }
```



- files 

- - 获取外部拖拽的文件，返回一个filesList列表
  - filesList下有个type属性，返回文件的类型

```js
oEle.ondragenter = function(ev){
     ev.preventDefault();  //浏览器会默认打开拖拽的文件
     var fs = ev.dataTransfer.files;   //获取到拖拽的文件
     //console.log(fs[0].type);  //文件的类型
 }
```



**FileReader**

> readAsDataURL

- 参数为要读取的文件对象，将文件读取为DataUrl 



> onload

- 当读取文件成功完成的时候触发此事件
- this. result , 来获取读取的文件数据，如果是图片，将返回base64格式的图片数据

```js
oEle.ondragenter = function(ev){
    var fd = new FileReader();
    fd.readDataURL(fs[0]);
    fd.onload = function(){  //读取成功
        if(fs[0].type.indexOf('image')!=-1){  //判断是否是图片类型
            console.log(this.result)
            var oLi = document.createElement('li');
            var oImg = document.createElement('img');
            
            oImg.src = this.result;  //预览图片
           oLi.appendChild(oImg);
           oUl.appendChild(oLi);
        } 
    }
}
```

实例：拖拽删除列表 拖拽购物车 上传图片预览功能 



**跨文档消息通信**

> postMessage对象

- 接受消息的窗口对象.postMessage();

- - 一参：发送的数据；二参：接收的域
  - postMessage方法可以给另一个窗口发送消息



- 交互方式

- - iframe:父页面ele.contentWindow（获取到被包含页面的window对象）、子页面：window.top
  - 窗口页：父页面window.open（返回的值是被打开窗口的window对象）、子页面window.opener（同域的情况下操作打开 当前页面的window）



- 接收事件

- - message  事件的事件对象下保存了发送过来的数据
  - ev.oriigin:发送数据来源的域
  - ev.data:发送的数据

—— 跨域访问的时候可以获取返回的对象，但是不能对页面元素的进行操作 

```js
ele.contentWindow.postMessage("发送的消息","跨域的域名http://www.yomi.com")  //跳转后的页面 
window.addEvenListener('message',function(ev){     
    console.log(ev.data) 
},false)  
```



- window表示当前窗口、top是顶层页面、parent是父级页面
  - 在iframe包含的页面通过parent.document.~~可以操作父级页面的元素 



**Ajax的跨域**

- 主流浏览器提供XMLHttpRequest对象支持更多特性，支持跨域请求 。但想实现跨域需后台配合，在后台添”herder(‘Access-Control-Allow-Origin: hhtp://域名’)“  //设置允许访问资源的域
- IE 6不支持该对象，要使用IE浏览器的特定实现ActiveXObject



**XMLHttpRequest Level 2**

- XMLHttpRequest改进版：增加了特性，推荐onload代替onreadystatechange

- - 请求页面与数据页面必须属性不同的域
  - 服务器要设置响应头信息
  - Origin值展现
  - IE在跨域请求下使用的对象：XDomainRequest（）
  -  新的事件：onload（代替onreadystatechange）

var getXmlHttpRequest = function () {     try{         //主流浏览器提供了XMLHttpRequest对象         return new XMLHttpRequest();     }catch(e){         //低版本的IE浏览器没有提供XMLHttpRequest对象，IE6以下         //所以必须使用IE浏览器的特定实现ActiveXObject         return new ActiveXObject("Microsoft.XMLHTTP");     } };  var xhr = getXmlHttpRequest();     // readyState 0=>初始化 1=>载入 2=>载入完成 3=>解析 4=>完成     // console.log(xhr.readyState);  0 xhr.open("TYPE", "URL", true); //xhr.open("post","url","是否异步上传"); //设置为post请求的时必须要设置 xhr.setRequestHeader('X-Request-With','XMLHttpRequest'); // console.log(xhr.readyState);  1  xhr.send(); //post提交时设置提交数据 // console.log(xhr.readyState);  1  xhr.onreadystatechange = function () {     // console.log(xhr.status); //HTTP状态吗     // console.log(xhr.readyState);  2 3 4     if(xhr.readyState === 4 && xhr.status === 200){        alert(xhr.responseText);     } }; 



**Ajax无刷新上传**

- - 进度事件 var OUpLoad = xhr.upload;

  - - upload.onprogress:上传进度  //要将onprogress放在上传事件之前

  - FromData对象

  - ev.total（已发送的总量）、ev.,loaded（待发送的总量）  //得出上传进度

  - onprogress:下载

<body>
    <input type="file" id="test-file"/>
    <input type="submit" value="选择的文件列表" id="btn"/>

    <script>
        var testFile = document.getElementById('test-file');
        var btn = document.getElementById('btn');

        //通过ajax要发送的信息是testFile.files[0]   //具体的文件对象
        btn.onclick = function(){
            console.log(testFile.files)   //返回控件testfile中选择的文件[object list]
            var xhr = new XMLHttpRequest();

            var oUpload = xhr.upload;   //要将onprogress放在上传事件之前
            oUpload.onprogress = function(ev){
                var iScale = ev.loaded/ev.total;
                btn.value = iScale*100;
                console.log(iScale*100)
                //进度条总长*iScale = 当前进度  iScale*100 
            }

            xhr.open('post','post_file.php',true);
            xhr.setRequestHeader('X-Request-with','XMLHttpRequest');
            //发送的文件
            var formData = new FormData();   //文件对象要用formData发送

            for(var attr in testFile.files){
                console.log(testFile.files[attr])   //文件对象：name、size、type等
                formData.append('file',testFile.files[attr]);   //key value  (key对应后台请求的数据参数)
            }

            xhr.send(formData);
            xhr.onload = function(){
                if(xhr.readyState=='4'&&xhr.status=='200'){
                    var backData = JSON.parse(this.responseText);
                    console.log(backData);
                }
            } 
        }
    </script>
</body>







**地理信息与本地存储**

**地理位置**

地理位置信息来源

- IP地址
- GPS全球定位系统
- WIFI无线网络
- 基站



地理位置对象

- navigator.geolocation （非标准IE不支持）

- - 单次定位请求  ：getCurrentPosition(请求成功，请求失败，数据收集方式)
  - 请求成功函数

var timer = navigator.getolocation.getCurrentPosition(function(position){     console.log(position.coords.longitude);   //打印出经度 },function(err){     console.log(ree.code)  //请求失败的编号 },{     enableHighAcuracy:true,  //更精确的查找     timeout:5000   //单位四毫秒 });  经度 :  coords.longitude 纬度 :  coords.latitude 准确度 :  coords.accuracy //下面四个必须是支持GPS定位的设备才可以获取到 海拔 :  coords.altitude 海拔准确度 :  coords.altitudeAcuracy 行进方向 :  coords.heading 地面速度 :  coords.speed 时间戳 : new Date(position.timestamp) 



- 请求失败函数

失败编号  ：code 0  :  不包括其他错误编号中的错误 1  :  用户拒绝浏览器获取位置信息 2  :  尝试获取用户信息，但失败了 3  :   设置了timeout值，获取位置超时了 



- 数据收集 :  json的形式

enableHighAcuracy  :  更精确的查找，默认false timeout  :  获取位置允许最长时间，默认infinity maximumAge :  位置可以缓存的最大时间，默认0 



- 多次定位请求  :  watchPosition(像setInterval)

- - 移动设备比较有用，因为位置改变才会触发
  - 配置参数：frequency 更新的频率 frequency:1000  //每秒



- 关闭更新请求  :  clearWatch(像clearInterval)

navigator.geolocation.clearWatch(timer) 



百度地图API

- <script src="http://api.map.baidu.com/api?v=1.3"></script>



**本地存储**

Cookie

- 数据存储到计算机中，通过浏览器控制添加与删除数据

- Cookie的特点

- - 存储限制

  - - 域名100个cookie,每组值大小4KB

  - 客户端、服务器端，都会请求服务器（头信息）

  - 页面间的cookie是共享



Storage

- sessionStorage

- - session临时回话，从页面打开到页面关闭的时间段
  - 窗口的临时存储，页面关闭，本地存储消失

- localStorage

- - 永久存储（可以手动删除数据）



Storage的特点

- 存储量限制 ( 5M )
- 客户端完成，不会请求服务器处理
- sessionStorage数据是不共享、 localStorage共享



Storage API

- setItem():

- - 设置数据，key\value类型，类型都是字符串
  - 可以用获取属性的形式操作

- getItem():

- - 获取数据，通过key来获取到相应的value

- removeItem():

- - 删除数据，通过key来删除相应的value

- clear():

- - 删除全部存储的值

window.sessionStorage.setItem('name','yomi'); window,sessionStorage.getItem('name'); window,sessionStorage.removeItem('name'); window,sessionStorage.clear(); 



存储事件:

- 当数据有修改或删除的情况下，就会触发storage事件

- - 在对数据进行改变的窗口对象上是不会触发的
  - Key : 修改或删除的key值，如果调用clear(),key为null
  - newValue  :  **新设置的值**，如果调用removeStorage(),key为null
  - oldValue :  调用改变前的value值
  - storageArea : 当前的storage对象
  - url :  触发该脚本变化的文档的url
  - 注：session同窗口才可以,例子：iframe操作

window.addEventListener('storage',function(ev){     //对当前页面window进行修改，不会触发当前窗口的该事件，只会在共享页面触发     if(ev.key == '存储的key值'){         if(ev.newValue = aInput[0].value){             aInput[0].checked = true;    //多页面数据同步更新         }     } },false); 







**HTML5音频与视频**

标签

- audio、video
- source



视频容器

- 音频轨道
- 视频轨道
- 元数据：封面，标题，字幕
- 格式：.avi、.flv、.mp4、.mkv、.ogv等



编解码器 （浏览器内嵌）

- 原始的视频容器非常大，添加需编码，播放需解码

- 音频编解码器

- - AAC、MPEG-3、Ogg Vorbis

- 视频编解码器

- - H.264、VP8、Ogg Theora



**媒体元素 （属性）**

- controls  :   显示或隐藏用户控制界面
- autoplay  :  媒体是否自动播放
- loop  : 媒体是否循环播放
- currentTime  :  开始到播放现在所用的时间
- duration  :  媒体总时间(只读)
- volume  :   0.0-1.0的音量相对值
- muted  :   是否静音    //设置静音就是将volume设置为0，在切换的时候将其设置为1
- autobuffer  :   开始的时候是否缓冲加载，autoplay的时候，忽略此属性

setInterval(function(){     console.log(ele.currentTime)  //可以通过该属性设置当前播放的时间 },1000); 

- paused  :   媒体是否暂停(只读)
- ended   :   媒体是否播放完毕(只读)
- error   :  媒体发生错误的时候，返回错误代码 (只读)
- currentSrc  :   以字符串的形式返回媒体地址(只读)



- play()  :  媒体播放    //ele.paly()调用播放
- pause()  :  媒体暂停
- load()  :  重新加载媒体    // 当修改了source中的src之后需要重新加载



媒体元素其它事件

- loadstart progress suspend emptied stalled play pause loadedmetadata loadeddata waiting playing canplay canplaythrough seeking seeked timeupdate ended ratechange durationchange volumechange

ele.addEventListener('ended',function(){     //视频播完之后触发 },false); 



video额外特性

- poster  :   视频播放前的预览图片  ele.poster = 'poster.png'
- width、height  :   设置视频的尺寸
- videoWidth、 videoHeight  :   视频的实际尺寸(只读)



//带声音的导航 window.onload = function(){ 	var aLi = document.getElementsByTagName('li'); 	var oA = document.getElementById('a1');  //audio不添加contrls是不可见的 	 	for(var i=0;i<aLi.length;i++){         aLi[i].onmouseover = function(){             //this.getAttribute('au');   //用au自定义不同属性以播放不同的音乐             oA.src = 'http://s8.qhimg.com/share/audio/piano1/'+this.getAttribute('au')+'4.mp3';             oA.play();         }; 	} }; 



//结合视频与canvas var oV = document.getElementById('v1'); var oC = document.getElementById('c1'); var oGC = oC.getContext('2d');  oC.width = oV.videoWidth; oC.height = oV.videoHeight;  setInterval(function(){ oGC.drawImage( oV , 0 , 0 );   //让canvas内容和视频保持一致 },30); 







**自制播放器**

window.onload = function(){     var oV = document.getElementById('v1');     var aInput = document.getElementsByTagName('input');     var timer = null;          //播放暂停键     aInput[0].onclick = function(){         if( oV.paused ){             oV.play();             this.value = '暂停';             nowTime();             timer = setInterval(nowTime,1000);         }         else{             oV.pause();             this.value = '播放';             clearInterval(timer);         }     };          //静音与否     aInput[2].value = changeTime(oV.duration);     aInput[3].onclick = function(){              if( oV.muted ){             oV.volume = 1;             this.value = '静音';                       oV.muted = false;                    }         else{                        oV.volume = 0;                       this.value = '取消静音';                         oV.muted = true;                     }            };          //点击全屏     aInput[4].onclick = function(){         oV.mozRequestFullScreen();   //firefox         oV.webkitRequestFullScreen();   //chrome     };          //设置当前播放的时长     function nowTime(){         aInput[1].value = changeTime(oV.currentTime);         //播放时根据时间长设置拖拽条的位置         var scale = oV.currentTime/oV.duration;           oDiv2.style.left = scale * (oDiv1.offsetWidth - oDiv2.offsetWidth)+ 'px';     }          //将毫秒数换成  hh:mm:ss     function changeTime(iNum){         iNum = parseInt( iNum );         var iH = toZero(Math.floor(iNum/3600));         var iM = toZero(Math.floor(iNum%3600/60));         var iS = toZero(Math.floor(iNum%60));         return iH + ':' +iM + ':' + iS;     }      //设置个位数时补0     function toZero(num){         if(num<=9){             return '0' + num;         }         else{             return '' + num;         }     } }; 



//添加拖拽进度条

//视频进度条拖拽     oDiv2.onmousedown = function(ev){         var ev = ev || window.event;         disX = ev.clientX - oDiv2.offsetLeft;  //拖拽条此时的left                  document.onmousemove = function(ev){             var ev = ev || window.event;             var L = ev.clientX - disX;              if(L<0){                 L = 0;             }             else if(L>oDiv1.offsetWidth - oDiv2.offsetWidth){                 L = oDiv1.offsetWidth - oDiv2.offsetWidth;  //靠最右边             }                          oDiv2.style.left = L + 'px';              var scale = L/(oDiv1.offsetWidth - oDiv2.offsetWidth);              oV.currentTime = scale * oV.duration;   //改变当前播放时间             nowTime();   //改变当前播放时间的显示效果                      };         document.onmouseup = function(){             document.onmousemove = null;         };         return false;     };          //音频进度条拖拽     oDiv4.onmousedown = function(ev){         var ev = ev || window.event;         disX2 = ev.clientX - oDiv4.offsetLeft;                  document.onmousemove = function(ev){             var ev = ev || window.event;                          var L = ev.clientX - disX2;                          if(L<0){                 L = 0;             }             else if(L>oDiv3.offsetWidth - oDiv4.offsetWidth){                 L = oDiv3.offsetWidth - oDiv4.offsetWidth;             }                          oDiv4.style.left = L + 'px';             var scale = L/(oDiv3.offsetWidth - oDiv4.offsetWidth);             oV.volume = scale;    //根据拖拽条比例设置音量                      };         document.onmouseup = function(){             document.onmousemove = null;         };         return false;     }; 