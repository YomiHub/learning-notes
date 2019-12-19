### Express框架的基本使用

> 环境：nodev12.10.0
- 参考文档：[Express](http://www.expressjs.com.cn/)
- ajax发送http请求[jquery3.1.0](https://github.com/jquery/jquery/releases/tag/3.1.0)

```html
<script type="text/javascript" src="jquery.min.js"></script>
  <script>
    $(function () {
      $('#login').click(function () {
        var dataObj = {
          username: $('input[name = "username"]').val(),
          pass: $('input[name = "pass"]').val()
        }
        $.ajax({
          type: 'POST',
          url: 'http://127.0.0.1:3000/login',
          contentType: 'application/json',
          dataType: 'text',
          //data:$('form:eq(0)').serialize(),
          data: JSON.stringify(dataObj),
          success: function (data) {
            console.log(data);
          }
        })
      })
    })
  </script>
```

#### 静态服务器
- `npm -init -y` 初始化项目
- `npm install express --save` 安装Express@4.17.1
- `const express = require('express'); const app = express();`引用的模块
```js
const app = require('express')();

app.get('/', (req, res) => {
  res.send('Hello World');   //测试：浏览器访问地址http://127.0.0.1:3000
}).listen(3000, () => {
  console.log('running···');
})
```

- [静态托管文件](http://www.expressjs.com.cn/starter/static-files.html)
  + use方法的第一个参数可以指定虚拟路径

```js
const express = require('express');
const path = require('path');
const app = express();

//静态托管文件
app.use(express.static(path.join(__dirname, 'public'))); //127.0.0.1:3000/public.html
app.use('/static', express.static(path.join(__dirname, 'images')));    //127.0.0.1:3000/static/static.png
app.listen(3000, () => {
  console.log('running');
});
```

#### 路由
> 根据请求路径和请求方式进行路径分发处理
- 常用的http请求方式：post(添加)、get(查询)、put(更新)、delete(删除)
- 直接使用use方法可以处理所有的路由请求

```js
app.get('/', function (req, res) {
  res.send('get');
})

app.post('/', function (req, res) {
  res.send('post');
})

app.delete('/user', function (req, res) {
  res.send('delete');
})

app.put('/user', function (req, res) {
  res.send('put')
})

app.listen(3000, () => {
  console.log("running");
})

/* app.use((req, res) => {
  res.send('use分发可以处理所有的路由请求');
}) */
```

#### 中间件
> 处理过程中的一个环节

- Express应用程序本质上是一系列中间件函数调用，可参考[中间件的使用](http://www.expressjs.com.cn/guide/using-middleware.html)
- next()将控制权传递给下一个中间件功能，可使用参数route跳过其余中间件
- 中间件的挂载方式：use、路由方式（get、post、put、delete）

> 应用层中间件：

```js
app.use((req, res, next) => {   //没有设置路径，即应用收到请求时都会执行该功能
  console.log('request type:', req.method);
  next();   //通过next传递，否则无法执行下一个中间件的功能
})

app.use('/user', (req, res, next) => {
  console.log('time:', Date.now());
  next('route');
}, (req, res) => {
  res.send("使用next('route')会跳转到下一个路由，不会执行该next");
})

app.get('/user', (req, res, next) => {
  res.send('next将控制权传递给该中间件功能');  //测试：访问http://127.0.0.1:3000/user
})


app.listen(3000, () => {
  console.log("running");
})
```

```js

//还可以使用回调的方式来调用中间件
var cb0 = function (req, res, next) {
  console.log('cb0');
  next();
}

app.get('/user', [cb0, cb1, cb2]);   //按照数组顺序访问函数cb
```

> 路由级中间件
- 路由器级中间件与应用程序级中间件的工作方式相同，只不过它绑定到的实例express.Router()
- 注意：需要将路由安装到app上

```js
const router = express.Router();

router.use((req, res, next) => {
  console.log('time:', Date.now());
  next();
})

router.use('/user', (req, res, next) => {
  console.log('request url', req.originalUrl);
  next();
})

router.get('/user', (req, res, next) => {
  res.send('res.render用来渲染模板文件,send()只能用一次，和end一样')
})

app.use('/', router); // mount the router on the app

app.listen(3000, () => {
  console.log("running");
})
```

> 错误处理中间件
- 错误处理中间件始终采用四个参数，使用上基本与其他中间件函数的定义方式相同
- 必须捕获由路由处理程序或中间件调用的异步代码中发生的错误，如果不使用try...catch则Express不会捕获该错误，因为它不是同步处理程序代码的部分

```js
app.get("/", function (req, res, next) {
  fs.readFile("/file-does-not-exist", function (err, data) {
    if (err) {
      next(err); // Pass errors to Express.
    }
    else {
      res.send(data);
    }
  });
});

app.use((err, req, res, next) => {
  console.log(err.stack);  //Error: ENOENT: no such file or directory, open 'e:\file-does-not-exist'
  res.status(500).send('someting broke!')
})

app.listen(3000, () => {
  console.log("running");
})
```

```js
//异步代码错误捕获
app.get('/', (req, res, next) => {
  setTimeout(function () {
    try {
      throw new Error('broken');
    } catch (err) {
      next(err);
    }
  }, 1000)
})

//使用Promise可以避免try...catch的开销
/* app.get('/', (req, res, next) => {
  Promise.resolve().then(function () {
    throw new Error("BROKEN");
  }).catch(next); // Errors will be passed to Express.
}) */


app.use((err, req, res, next) => {
  console.log(err.stack);
  res.status(500).send('someting broke!')
})

app.listen(3000, () => {
  console.log("running");
})
```

> 内置中间件
- express.static提供静态资源，例如HTML文件，图像等
- express.json使用JSON负载解析传入的请求。注意：Express 4.16.0+中可用
- express.urlencoded使用URL编码的有效内容解析传入的请求。 注意：Express 4.16.0+中可用

> 第三方中间件
- 在应用程序级别或路由器级别将其加载到Express应用程序，为其添加功能
- 常用的[第三方中间件](http://www.expressjs.com.cn/resources/middleware.html)
- 案例：[body-parser](https://www.npmjs.com/package/body-parser)
`npm install body-parser --save`

```js
const express = require('express');
const app = express();
const bodyParser = require('body-parser');
//挂载内置中间件
app.use(express.static(path.join(__dirname, 'public')))

//挂载参数处理中间件body-parser
app.use(bodyParser.urlencoded({ extended: false }));

// parse application/json  当客户端传递的数据为Json格式需要添加如下设置
app.use(bodyParser.json());

app.get('/login', (req, res) => {
  console.log(req.query);
  res.send('get提交可以用query获取参数');
})

//使用中间件之后不需要进行对象转换，方便参数处理
app.post('/login', (req, res) => {
  let data = req.body;
  console.log(data);
  if (data.username == 'hym' && data.pass == "hym") {
    res.send('success')
  } else {
    res.send('failure')
  }
})

app.listen(3000, () => {
  console.log("running");
})

```



#### 模板引擎整合
-  使用art-template@4.0 新特性，[express-art-template]("https://github.com/aui/express-art-template") 以及  [koa-art-template]("https://github.com/aui/koa-art-template")
-  安装express-art-template使express兼容art-template模板引擎
```
npm install --save art-template
npm install --save express-art-template
```
- 创建app.js编写以下代码，在同级目录下创建文件夹views并创建模板文件list.art

```js

const express = require('express');
const path = require('path');
const template = require('art-template');
const app = express();

//设置模板路径
app.set('views', path.join(__dirname, './views'));

//设置模板后缀为art，与app.engine相对应
app.set('view engine', 'art');
app.engine('art', require('express-art-template'));   //express兼容art-template引擎
app.get('/list', (req, res) => {
  let data = {
    title: '水果',
    list: [
      '苹果',
      '猕猴桃',
      '圣女果'
    ]
  };
  res.render('list', data);  //指定文件名称，渲染数据
})

//测试：访问http://localhost:3000/list
app.listen(3000, () => {
  console.log("running");
})

```

```html
<body>
  <div>
    <ul>
      {{each list}}
      <li>{{$value}}</li>
      {{/each}}
    </ul>

  </div>
</body>
```