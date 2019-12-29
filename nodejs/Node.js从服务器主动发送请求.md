### Node.js从服务器主动发送请求

#### 服务器请求百度首页
- 参考文档[链接](https://nodejs.org/dist/latest-v12.x/docs/api/http.html#http_http_request_url_options_callback)

```js
//服务器发送请求
const http = require('http');
const path = require('path');
const fs = require('fs');

let options = {
  hostname: 'www.baidu.com',
  port: '80'
}

let req = http.request(options, (res) => {
  let info = '';
  res.on('data', (chunk) => {
    info += chunk;
  })

  res.on('end', () => {
    fs.writeFile(path.join(__dirname, 'baidu.html'), info, (err) => {
      if (!err) {
        console.log("百度主页html内容加载完毕")
      }
    })
  })
})


req.end();
```

#### 服务器向后台接口发送请求
- 查询数据

```js
const http = require('http');

let options = {
  protocol: 'http:',
  hostname: 'localhost',
  port: '3000',
  path: '/books'
}

let req = http.request(options, (res) => {
  let info = '';
  res.on('data', (chunk) => {
    info += chunk;
  })

  res.on('end', () => {
    console.log(info);
  })
})

req.end();
```

- 添加数据
  + 需要在headers中指定Content-Type
  + 需要引入querystring模块，对请求的参数对象转成字符串进行处理
  + 需要调用req的write(data)方法将请求的参数写入请求体中

```js
//服务器向添加图书resful接口发送带参请求
const http = require('http');
const querystring = require('querystring');

let options = {
  protocol: 'http:',
  hostname: 'localhost',
  port: '3000',
  path: '/books/book',
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
};

//对应添加数据的字段和值
let addData = querystring.stringify({
  'name': '书名',
  'author': '作者',
  'category': '分类',
  'description': '描述'
});

let req = http.request(options, (res) => {
  let info = '';
  res.on('data', (chunk) => {
    info += chunk;
  })

  res.on('end', () => {
    console.log(info);  //打印接口返回的信息
  })
})

req.on('error', (e) => {
  console.error(`problem with request: ${e.message}`);
});

req.write(addData);
req.end();
```

- 带参查询数据
  + 在请求路径后拼接参数Id（请求的后台接口是resful接口）

```js
//带参查询
const http = require('http');

let id = 9;
let options = {
  protocol: 'http:',
  hostname: 'localhost',
  port: '3000',
  path: '/books/book/' + id,
  method: 'GET'
};

let req = http.request(options, (res) => {
  let info = '';
  res.on('data', (chunk) => {
    info += chunk;
  })

  res.on('end', () => {
    console.log(info);
  })
})

req.on('error', (e) => {
  console.error(`problem with request: ${e.message}`);
});

req.end();
```

- 修改数据
  + 与添加数据基本一致，只需要更改请求方式为put以及参数中添加修改的数据id

```js
let options = {
  protocol: 'http:',
  hostname: 'localhost',
  port: '3000',
  path: '/books/book',
  method: 'PUT',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  }
};

//对应添加数据的字段和值
let addData = querystring.stringify({
  'id': 18,
  'name': '书名修改',
  'author': '作者修改',
  'category': '分类修改',
  'description': '描述修改'
});
```

- 删除数据
  + 删除数据与带参查询数据一致，只需要更改请求方式为delete

```js
let id = 18;
let options = {
  protocol: 'http:',
  hostname: 'localhost',
  port: '3000',
  path: '/books/book/' + id,
  method: 'delete'
};

```

#### 请求第三方服务器的后台接口
> 封装天气查询模块

```js
//weather.js
const http = require('http');

//http://www.weather.com.cn/data/sk/101010100.html   通过拼接城区Id查询
exports.queryWeather = (cityCode, callback) => {
  let options = {
    protocol: 'http:',
    hostname: 'www.weather.com.cn',
    port: '80',
    path: '/data/sk/' + cityCode + '.html',
    method: 'get'
  };

  let req = http.request(options, (res) => {
    let info = '';
    res.on('data', (chunk) => {
      info += chunk;
    })

    res.on('end', () => {
      info = JSON.parse(info);
      callback(info);
    })
  })

  req.on('error', (e) => {
    console.error(`problem with request: ${e.message}`);
  });

  req.end();
}

```

> 测试封装模块

```js
//test.js
const weather = require('./weather.js')

weather.queryWeather('101020100', (data) => {
  console.log(data);  //打印出对应ID的天气信息
})
```