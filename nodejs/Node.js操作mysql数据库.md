### Node.js操作mysql数据库

#### 依赖的包
- nodejs操作数据库需要依赖的包mysql，参考文档[连接](https://github.com/mysqljs/mysql) 

</br>

#### mysql包的基本使用
- 创建项目，初始化项目`npm init -y`
- 安装mysql包(最新版本) `npm install mysqljs/mysql`
- 创建connectDB.js文件，用以下代码测试能否正确连接数据库，正常连接时输出 'The solution is:  2'

```js
//加载数据库驱动
const mysql = require('mysql');

//创建数据库连接
const connection = mysql.createConnection({
  host: 'localhost',  //数据库所在服务器的ip或者域名
  user: 'root',  //登录数据库的账号
  password: 'rootpass',   //登录数据库的密码
  database: 'library'  //连接的数据库名称
});

connection.connect();  //执行连接操作

//操作数据库
connection.query('SELECT 1 + 1 AS solution', function (error, results, fields) {
  if (error) throw error;
  console.log('The solution is: ', results[0].solution);
});

//关闭连接
connection.end();
```

</br>

#### 插入操作（增）

```js
const mysql = require('mysql');
const connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'rootpass',
  database: 'library'
});

connection.connect();

//执行插入操作
let sql = 'insert into book set ?';
let data = {
  name: '图书名称',
  author: '图书作者',
  category: '图书分类',
  description: '图书简述'
}
connection.query(sql, data, function (error, results, fields) {
  if (error) throw error;
  if (results.affectedRows == 1) { console.log('插入成功') };
  //执行插入成功返回的results
  // OkPacket {
  //   fieldCount: 0,
  //   affectedRows: 1,
  //   insertId: 7,
  //   serverStatus: 2,
  //   warningCount: 0,
  //   message: '',
  //   protocol41: true,
  //   changedRows: 0
  // }
});

connection.end();
```

</br>

#### 删除操作（删）

```js
const mysql = require('mysql');
const connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'rootpass',
  database: 'library'
});

connection.connect();

//执行删除操作
let sql = 'delete from book where id=?';
let data = [7];
connection.query(sql, data, function (error, results, fields) {
  if (error) throw error;
  if (results.affectedRows == 1) {
    console.log('删除成功');
  }
})
connection.end();
```

</br>

#### 查询操作（查）

```js
const mysql = require('mysql');
const connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'rootpass',
  database: 'library'
});

connection.connect();

//查询操作
let sql = 'select * from book';
let data = null;
connection.query(sql, data, function (error, results, fields) {
  if (error) throw error;
  console.log(results);  //返回对象数组，results[0]获取第一条
})
connection.end();
```

</br>

#### 更新操作（改）

```js
const mysql = require('mysql');
const connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'rootpass',
  database: 'library'
});

connection.connect();


//执行更新操作
let sql = 'update book set name=?,author=?,category=?,description=? where id=?';
let data = ['巨人的陨落', '肯·福莱特', '小说', '描述了在第一次世界大战时，发生在一些人身上的故事', 7];
connection.query(sql, data, function (error, results, fields) {
  if (error) throw error;
  if (results.affectedRows == 1) {
    console.log('更新成功');
  }
})

connection.end();
```

</br>

#### mysql数据库通用操作封装

```js
//connectDB.js
const mysql = require('mysql');

exports.base = (sql, data, callback) => {
  const connection = mysql.createConnection({
    host: 'localhost',
    user: 'root',
    password: 'rootpass',
    database: 'library'
  });

  connection.connect();

  connection.query(sql, data, function (error, results, fields) {
    if (error) throw error;
    callback(results);  //数据库操作是异步的，所以通过回调函数返回操作结果
  })
  connection.end();
}
```

测试：
```js
//testDB.js
const db = require('./connectDB.js');

let sql = 'insert into book set ?';
let data = {
  name: '图书名称',
  author: '图书作者',
  category: '图书分类',
  description: '图书简述'
}

db.base(sql, data, (results) => {
  console.log(results);
})
```