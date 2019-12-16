### Node.js实现服务器配置

#### Node.js 实现静态网站功能
- 使用http模块初步实现服务器功能

```js
const http = require('http');

let server = http.createServer();
server.on('request', (req, res) => {
  res.end("hello world");  //访问浏览器地址127.0.0.1:3000在网页上显示hello world
});
server.listen(3000); //设置监听的端口
```

- 实现静态服务器功能
  + 实现请求路径的分发
    - req对象是[Class: http.IncomingMessage](https://nodejs.org/dist/latest-v12.x/docs/api/http.html#http_class_http_incomingmessage)的实例对象，可以在该类下查看相关方法的使用
    - res对象是[Class: http.ServerResponse](https://nodejs.org/dist/latest-v12.x/docs/api/http.html#http_class_http_serverresponse)的实例对象
  + 实现图片等其他资源的访问需要根据文件类型设置响应类型

```js
const http = require('http');
const fs = require('fs');
const path = require('path');
const mime = require('./lib/mime.json');  //用于通过文件后缀设置响应头

//用于读取指定路径的文件内容并返回到浏览器
let readFile = (dpath, res) => {
  fs.readFile(path.join(__dirname, 'www', dpath), (err, fileContent) => {
    if (err) {
      //第一个参数为响应状态码
      res.writeHead(404, {
        'Content-Type': 'text/plain;charset=utf8'//设置响应类型和编码，解决乱码
      })
      res.end("404页面找不到了");//终止响应，不可以多次写入
    } else {
      let dtype = "text/html";  //设置默认类型
      let ext = path.extname(dpath);
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


http.createServer((req, res) => {
  //res.end(req.url);  //获取到端口之后的路径
  if (req.url.startsWith('/index')) {
    //res.write("访问了");  //向客户端响应内容，可以写入多次
    readFile(req.url, res);
  } else if (req.url.startsWith('/list')) {
    readFile(req.url, res);
  } else if (req.url.startsWith('/detail')) {
    readFile(req.url, res);
  }
  
  //以上的if判断可以省略，直接调用 readFile(req.url, res); 即可
}).listen(3000, '192.168.1.104', () => {  //该ip为本电脑连接wifi后的IP地址，可省略则默认为127.0.0.1
  console.log("服务running");
})
```
</br>
-----



#### 参数传递与获取
- [get参数获取](https://nodejs.org/dist/latest-v12.x/docs/api/url.html#url_url_parse_urlstring_parsequerystring_slashesdenotehost)
    + url.parse(urlString) 将url字符串转为对象，第二个参数可传入true表示将对参数url进行querystring.parse解析
    + url.format(urlObject)  将url对象转为字符

```js
const url = require('url');
let myURL =
  url.parse('http://user:pass@sub.example.com:8080/p/a/t/h?query=string#hash');
console.log(myURL);
url.format(myURL);
/*
Url {
  protocol: 'http:', //协议
  slashes: true,  //当Url协议中的冒号后面包含双斜杠/则为true
  auth: 'user:pass',  //用户名密码认证信息
  host: 'sub.example.com:8080', //包含端口号的域名
  port: '8080',   //端口
  hostname: 'sub.example.com',    //不包含端口号的域名
  hash: '#hash',  //锚点
  search: '?query=string',   //问号以及所有参数
  query: 'query=string',   //去掉问号后的所有参数
  pathname: '/p/a/t/h',     //域名后之后不包含参数的路径
  path: '/p/a/t/h?query=string',    //包含参数的路径
  href: 'http://user:pass@sub.example.com:8080/p/a/t/h?query=string#hash'  //url完整字符串
}
*/
```

- get请求处

```js
const http = require('http');
const ss = require('./staticServer.js');
const path = require('path');
const url = require('url');

http.createServer((req, res) => {
  let urlObj = url.parse(req.url, true);
  res.end(urlObj.query.username + '-' + urlObj.query.pass);
}).listen(3000, () => {
  console.log("服务running");
})

//测试：http://127.0.0.1:3000/index.html?username=hym&pass=12
```
</br>



- [post参数获取](https://nodejs.org/dist/latest-v12.x/docs/api/querystring.html#querystring_query_string)
  + querystring.parse(urlstr)  将字符串转为对象，当多个参数一样时，将参数的值放入数组中
  + querystring.stringify(obj) 将对象中的参数拼接成字符串（自动拆分数组）

```js
const querystring = require('querystring');
let param = 'foo=bar&abc=xyz&abc=123';
let obj = querystring.parse(param);
console.log(obj);
/* {
  foo: 'bar',
  abc: ['xyz', '123']
} */

let str = querystring.stringify(obj);
console.log(str);
/* foo=bar&abc=xyz&abc=123 */
```

- post请求处理

```js
const http = require('http');
const querystring = require('querystring');
http.createServer((req, res) => {
  if (req.url.startsWith('/login')) {
    let allData = '';
    req.on('data', (chunk) => {
      allData += chunk;    //将接受的数据拼接完整
    });

    req.on('end', () => {
      console.log(allData);
      let obj = querystring.parse(allData);
      res.end(obj.username + "-" + obj.pass);
    })
  }
}).listen(3000, () => {
  console.log("服务running···");
})

//测试：使用chrome插件postman模拟发送post请求,参数格式选择www-form-urlencoded
```
</br>
------
#### 动态网站
> 创建服务器实现动态网站的效果
- 路由 即请求路径+请求方式
- .tpl 模板文件

```js
//visual studio code中将.tpl文件识别为html文件并高亮 
// 将设置放入file --> preferences --> settings
//-->Text Editor --> Files --> Edit in settings.json文件中以覆盖默认设置
{
    "files.associations": {
        "*.tpl":"html"
    }
}
```

```html
<!--创建模板文件query用于输入关键字、result.tpl用于存放查询的结果-->
<body>
  <table>
    <tr>
      <th>英文</th>
      <th>中文</th>
      <th>应用场景</th>
    </tr>
    <tr>
      <td>$$english$$</td>
      <td>$$chinese$$</td>
      <td>$$application$$</td>
    </tr>
  </table>
</body>
```

- node服务（需要准备虚拟数据search.json文件）

```js
const http = require('http');
const path = require('path');
const fs = require('fs');
const querystring = require('querystring');
const virtualData = require('./virtualData/search.json');

http.createServer((req, res) => {
  //查询入口 /query
  if (req.url.startsWith('/query') && req.method == 'GET') {
    fs.readFile(path.join(__dirname, 'www/view', 'query.tpl'), 'utf8', (err, content) => {
      if (err) {
        res.writeHead(500, {
          'Content-Type': 'text/plain;charset=utf8'
        });
        res.end("500服务器错误！")
      }

      res.end(content);
    })
  } else if (req.url.startsWith('/result')) {
    let allData = '';
    req.on('data', (chunk) => {
      allData += chunk;
    });

    req.on('end', () => {
      let obj = querystring.parse(allData);
      let result = virtualData[obj.key];
      fs.readFile(path.join(__dirname, 'www/view', 'result.tpl'), 'utf8', (err, content) => {
        if (err) {
          res.writeHead(500, {
            'Content-Type': 'text/plain;charset=utf8'
          });
          res.end("500服务器错误！")
        }

        //进行数据渲染
        content = content.replace('$$english$$', result.english);
        content = content.replace('$$chinese$$', result.chinese);
        content = content.replace('$$application$$', result.application);
        //或者使用模板引擎art-template
        //let html = template(path.join(__dirname, 'www/view', 'result.art'), result)
        res.end(content);
      })
    })
  }
}).listen(3000, () => {
  console.log("服务running···")
})
```

</br>

- 案例：实现登录验证功能

```js
const http = require('http');
const querystring = require('querystring');
const ss = require('./staticServer.js');

http.createServer((req, res) => {
  //启动静态资源服务
  if (req.url.startsWith('/www')) {
    ss.staticServer(req, res, __dirname);
  }

  //启动动态服务
  if (req.url.startsWith('/login')) {
    //get请求处理
    if (req.method == 'GET') {

    }
    //post请求处理
    if (req.method == 'POST') {
      let allData = '';
      req.on('data', (chunk) => {
        allData += chunk;
      });

      req.on('end', () => {
        let obj = querystring.parse(allData);
        res.writeHead(500, {
          'Content-Type': 'text/plain;charset=utf8'  //防止中文乱码
        });
        if (obj.username == 'admin' && obj.pass == 'root') {
          res.end('登录成功');
        } else {
          res.end('登录失败');
        }
      });
    }
  }
}).listen(3000, () => {
  console.log("服务running···")
}) 
```

<br>

-----

#### 模板引擎  [art-template](https://aui.github.io/art-template/zh-cn/docs/)
- 理解模板引擎本质
- 引擎基本使用

```js
const path = require('path');
var template = require('art-template');

//使用方式一：创建好.art文件
var html = template(path.join(__dirname, '../mytpl.art'), {
  user: {
    name: 'aui'
  }
});

console.log(html);

//使用方式二：字符串形式
/* let tpl = '<ul>{{each list value index}}<li>{{index}}{{value}}</li>{{/each}}</ul>';
let render = template.compile(tpl);
let ret = render({
  list: ['english', 'math', 'chinese']
});
console.log(ret); */

//使用方式三：简单化
/* let tpl = '<ul>{{each list}}<li>{{$index}}{{$value}}</li>{{/each}}</ul>';
let ret = template.render(tpl, {
  list: ['english', 'math', 'chinese']
});
console.log(ret); */
```