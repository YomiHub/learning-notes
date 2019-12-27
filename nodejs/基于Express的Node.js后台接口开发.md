### 基于Express的Node.js后台接口开发

#### 开发环境以及依赖的包
- 在本地安装依赖的包 `npm install express --save` 
- node操作数据库依赖的mysql包 `npm install mysqljs/mysql --save`
- 数据库通用操作封装connectDB.js，文件[链接](https://github.com/YomiHub/nodeLibraryManage/blob/master/libraryManage1/connectDB.js)
- mysql环境：Server version: 5.7.24 MySQL Community Server (GPL)
- node环境安装，这里用的版本是v12.10.0
- npm安装，这里使用的版本是6.10.0
- 参考文档：[express](http://www.expressjs.com.cn/guide/routing.html)、[mysql](https://github.com/mysqljs/mysql)

#### json接口

```js
const express = require('express');
const db = require('./connectDB.js');
const app = express();

app.get('/query', (req, res) => {
  let sql = 'select * from book';
  db.base(sql, null, (result) => {
    res.json(result); //访问127.0.0.1:3000/query  返回json格式的数据
  })
})

app.listen(3000, () => {
  console.log('服务启动');
})
```

#### jsonp接口
```js
const express = require('express');
const db = require('./connectDB.js');
const app = express();

//jsonp接口，将返回的json数据通过参数的形式返回，默认回调函数callback
//app.set('jsonp callback name', 'cb');  设置callback的名称为cb
app.get('/query', (req, res) => {
  let sql = 'select * from book';
  db.base(sql, null, (result) => {
    res.jsonp(result); 
    //访问127.0.0.1:3000/query?callback=fun  json数据作为fun函数的参数返回，请求参数callback名称错误则按json格式的数据返回
  })
})

app.listen(3000, () => {
  console.log('服务启动');
})
```

#### resful接口
- 从url的格式来表述（请求的方式影响请求的资源）
  + 页面渲染（查）get请求：http://localhost:3000/books
  + 页面跳转get请求：http://localhost:3000/books/book
  + 提交表单（增）post请求：http://localhost:3000/books/book
  + 根据id查询内容get请求：http://localhost:3000/books/book/1  （将传递的参数直接添加到url后面）
  + 更新内容（改）put请求：http://localhost:3000/books/book
  + 根据id删除内容delete请求：http://localhost:3000/books/book/2
- 可以通过json或者jsonp返回结果

```js
const express = require('express');
const db = require('./connectDB.js');
const app = express();

//resful接口
app.get('/books', (req, res) => {
  let sql = 'select * from book';
  db.base(sql, null, (result) => {
    res.json(result);
    //访问127.0.0.1:3000/query?callback=fun  json数据作为fun函数的参数返回，请求参数callback名称错误则按json格式的数据返回
  })
})

//带参的情况 http://localhost:3000/books/book/1
app.get('/books/book/:id', (req, res) => {
  let id = req.params.id;
  let sql = 'select * from book where id=?';
  let data = [id];
  db.base(sql, data, (result) => {
    res.json(result[0]);
  })
})

app.listen(3000, () => {
  console.log('服务启动');
})
```