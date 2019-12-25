#### 基于Node.js操作mysql数据库的通用api

```js
//connectDB.js
const mysql = require('mysql');

exports.base = (sql, data, callback) => {
  const connection = mysql.createConnection({
      host: 'localhost',  //数据库所在服务器的ip或者域名
      user: 'root',  //登录数据库的账号
      password: 'rootpass',   //登录数据库的密码
      database: 'library'  //连接的数据库名称
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

//插入操作
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

//删除操作
/* let sql = 'delete from book where id=?';
let data = [7];
db.base(sql, data, (results) => {
  if (results.affectedRows > 0) {
    console.log('删除成功');
  }
}) */

//查询操作
/* let sql = 'select * from book';
let data = null;
db.base(sql, data, (results) => {
  console.log(results);
}) */

//更新操作
/* let sql = 'update book set name=?,author=?,category=?,description=? where id=?';
let data = ['巨人的陨落', '肯·福莱特', '小说', '描述了在第一次世界大战时，发生在一些人身上的故事', 7];
db.base(sql, data, (results) => {
  if (results.affectedRows > 0) {
    console.log('更新成功');
  }
}) */
```