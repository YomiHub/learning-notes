### vue发起Ajax请求（v-resource）

#### vue-resource实现get、post、jsonp
> 除了 vue-resource 之外，还可以使用 `axios` 的第三方包实现实现数据的请求。关于vue-resource可以从[github](https://github.com/pagekit/vue-resource/releases)下载，这里使用的是1.5.1版本（注意在引入vue包之后再引入vue-resource）

> 具体请求参数可以参考[链接](https://github.com/pagekit/vue-resource/blob/develop/docs/http.md)
- 初始化页面的数据请求一般放到created()声明周期函数中调用 
- 可以在全局设置请求的 根域名，注意，当使用全局配置时，请求路径不能以斜杠'/'开头，而是用相对路径，否则不会启用路径拼接，详细可以参考[github](https://github.com/pagekit/vue-resource/blob/develop/docs/config.md)
- 可以在全局配置emulateJSON选项，避免每次发起post请求时都要在第三个参数设置内容类型 `Vue.http.options.emulateJSON = true;`

```html
  <script>
    Vue.http.options.root = 'http://127.0.0.1:3000/';
    //Vue.http.options.emulateJSON = true;
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {
        //本地后台代码见 https://github.com/YomiHub/nodeLibraryManage
        sendGet() {
          this.$http.get('books').then(function (result) {
            console.log(result.body);
          }, function (err) {

          })
        },
        postBook() {
          //发送post请求设置表单内容类型 application/x-www-form-urlencoded
          this.$http.post('books/book', {
            name: '书名 ',
            author: '作者',
            category: '分类',
            description: '描述'
          }, {
            emulateJSON: true //在第三个参数设置提交内容类型为普通表单数据格式
          }).then(result => {
            console.log(result.body);
          }, err => {

          })
        },
        sendJsonp() {
          this.$http.jsonp('books/jsonp').then(result => {
            console.log(result.body);
          }, err => {
            console.log(err)
          })
        }
      }

    })
  </script>
```

</br>

#### 跨域
> 由于浏览器的安全性限制，不允许AJAX访问 协议不同、域名不同、端口号不同的 数据接口，浏览器认为这种访问不安全

- jsonp解决跨域：可以通过动态创建script标签的形式，把script标签的src属性，指向数据接口的地址，因为script标签不存在跨域限制，这种数据获取方式，称作JSONP（注意：根据JSONP的实现原理，知晓，JSONP只支持Get请求）
  + 先在客户端定义一个回调方法，预定义对数据的操作；
  + 再把这个回调方法的名称，通过URL传参的形式，提交到服务器的数据接口；
  + 服务器数据接口组织好要发送给客户端的数据，再拿着客户端传递过来的回调方法名称，拼接出一个调用这个方法的字符串，发送给客户端去解析执行；
  + 客户端拿到服务器返回的字符串之后，当作Script脚本去解析执行，这样就能够拿到JSONP的数据了；

```js
//Node.js实现JSONP
const http = require('http');
// 导入解析 URL 地址的核心模块
const urlModule = require('url');
const server = http.createServer();

// 监听 服务器的 request 请求事件，处理每个请求
server.on('request', (req, res) => {
  const url = req.url;

  // 解析客户端请求的URL地址
  var info = urlModule.parse(url, true);

  // 如果请求的 URL 地址是 /getjsonp ，则表示要获取JSONP类型的数据
  if (info.pathname === '/getjsonp') {
    // 获取客户端指定的回调函数的名称
    var cbName = info.query.callback;
    // 手动拼接要返回给客户端的数据对象
    var data = {
      name: 'zs',
      age: 22,
      gender: '男',
      hobby: ['吃饭', '睡觉', '运动']
    }
    // 拼接出一个方法的调用，在调用这个方法的时候，把要发送给客户端的数据，序列化为字符串，作为参数传递给这个调用的方法：
    var result = `${cbName}(${JSON.stringify(data)})`;
    // 将拼接好的方法的调用，返回给客户端去解析执行
    res.end(result);
  } else {
    res.end('404');
  }
});

server.listen(3000, () => {
  console.log('server running at http://127.0.0.1:3000');
});
```


- Node.js后端解决跨域
  + node后台接口代码详情见[链接](https://github.com/YomiHub/nodeLibraryManage)

```js
//允许某些来源、某些接口、某些方法以某些形式被跨域调用。app.all(···)
router.all('*', function (req, res, next) {
  res.header("Access-Control-Allow-Origin", "*");
  res.header('Access-Control-Allow-Methods', 'PUT, GET, POST, DELETE, OPTIONS');
  res.header("Access-Control-Allow-Headers", "X-Requested-With");
  res.header('Access-Control-Allow-Headers', 'Content-Type');
  next();
});
```