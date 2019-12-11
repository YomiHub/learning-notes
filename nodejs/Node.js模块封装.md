### Node.js模块封装

##### 静态服务功能（将对应路径的文件内容返回到浏览器）
> 环境：node12.10.0、引入文件：mime.json 

```js
//staticServer模块
const fs = require('fs');
const path = require('path');
const mime = require('./lib/mime.json');  //用于通过文件后缀设置响应头
exports.staticServer = (req, res, rootpath) => {
  fs.readFile(path.join(rootpath, req.url), (err, fileContent) => {
    if (err) {
      //第一个参数为响应状态码
      res.writeHead(404, {
        'Content-Type': 'text/plain;charset=utf8'//设置响应类型和编码，解决乱码
      })
      res.end("404页面找不到了");//终止响应，不可以多次写入
    } else {
      let dtype = "text/html";  //设置默认类型
      let ext = path.extname(req.url);
      if (mime[ext]) {
        dtype = mime[ext];  //通过文件后缀获取对应类型
      }

      if (dtype.startsWith('text')) {
        dtype += ';charset=utf8';  //响应内容为文本的时候设置编码
      }
      res.writeHead(200, {
        'Content-Type': dtype
      })
      res.end(fileContent);
    }
  })
}
```
- 测试代码(启动服务，通过127.0.0.1:3000/index.html访问到项目目录下www目录中的文件内容)
```js
//server.js
const http = require('http');
const ss = require('./staticServer.js');
const path = require('path');

http.createServer((req, res) => {
  ss.staticServer(req, res, path.join(__dirname, 'www'));
}).listen(3000, () => {
  console.log("服务running");
})
```