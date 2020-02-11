### Promise

#### Promise解决回调
> 读取文件是异步操作，往往需要将读取结果通过回调函数的方式返回出去

- 当通过callback函数实现单个文件内容读取时

```js
const fs = require('fs');
const path = require('path');

//处理方式一：callback有两个参数，第一个是失败的结果，第二个是成功的结果，且成功时参数一位null，失败时参数二维undefined
//处理方式二：将两个参数拆分为两个回调函数successCallback与errorCallback分别处理结果
function readFileByPath (path, callback) {
  fs.readFile(path, 'utf-8', (error, dataStr) => {
    //不能直接throw抛出异常，需要将错误处理交出去
    if (error) return callback(error);  //注意这里使用return是在失败时不再执行下面的代码
    callback(null, dataStr);
  })
}

readFileByPath(path.join(__dirname, './testCallback.txt'), (error, dataStr) => {
  if (error) return console.log(error.message)
  console.log(dataStr);
})
```
- 如果需要按顺序读取多个文件内容，一般处理则需要嵌套多个回调（简称回调地域），这种情况可以用promise解决，但并不能减少代码量

#### Promise基本概念
- Promise是一个构造函数，可以通过new创建实例（console.dir(Promise)）
- 在Promise上有两个比较重要的函数resolve（成功之后的回调函数）和reject（失败之后的回调函数）
- Promise构造函数的prototype属性上有一个.then()方法，即Promise实例可以访问.then()方法
- 每当New一个Promise的实例则表示一个具体的异步操作
- Promise异步操作的结果只有两种状态
  + 状态1：异步执行成功，成功的回调resolve将结果返回给调用者
  + 状态2：异步执行失败，失败的回调reject将结果返回给调用者
  + 同样因为Promise的实例是异步操作，所以不能直接通过return返回结果，需要使用回调函数的形式将成功或者失败的结果返回给调用者
- 可以在new出来的Promise实例上调用.then()方法，【预先】为Promise异步操作指定resolve或reject回调函数

</br>

#### 使用Promise
- new Promise()只是形式上的异步操作，并不知道异步的具体操作；相对的，具体的Promise是指在实例中通过function进行具体的异步操作new Promise(function(){})
- 在使用new Promise的时候，除了会得到一个实例之外，还会立即执行构造函数中传递的function中的异步操作代码，所以如果希望调用才执行，就要将实例创建放在function中
- 在promise实例的异步操作中不能return，可以用resolve和reject处理结果，然后将promise对象返回
- promise实例通过调用then()顺序定义resolve和reject实参，在运行程序时先执行完then中的方法，异步操作才会执行完，所以就能在异步操作中调用resolve和reject

```js
const fs = require('fs');
const path = require('path');

function getFileByPath (fpath) {
  //new实例中的resolve和reject是形参，需要在then()方法中定义实参
  return new Promise(function (resolve, reject) {
    fs.readFile(path.join(__dirname, fpath), 'utf-8', (err, dataStr) => {
      if (err) return reject(err);
      resolve(dataStr);
    })
  })
}

getFileByPath('./testPromise.txt').then(function (data) {
  console.log(data);
}, function (err) {
  console.log(err.message);
})
```

#### Promise解决地狱回调的问题
> 注意：通过then指定回调函数时，第一个即成功的回调必须传，但是失败的回调可以不传

- 如果promise异步操作报错，且不希望影响后面异步操作的执行，则需要指定失败的回调函数，并返回下一个promise对象

```js
//通过.then方法返回新的promise对象，实现顺序读取多个文件
getFileByPath('./testPromise1.txt').then(function (data) {
  //then中可以只指定成功的回调
  console.log(data);
  return getFileByPath('./testPromise2.txt');  //返回新的promise对象
}, function (err) {
  //指定失败的回调，这样异步操作执行失败也不会对后续promise操作造成影响
  console.log(err.message);
  return getFileByPath('./testPromise2.txt');
}).then(function (data) {
  console.log(data);
})
```

- 如果后续promise执行的操作依赖于前面promise执行成功的结果，则可以在异步操作失败的时候就终止程序（注意不需要指定失败回调）

```js
getFileByPath('./testPromise1.txt').then(function (data) {
  console.log(data);
  return getFileByPath('./testPromise2.txt'); 
}).then(function (data) {
  console.log(data);
}).catch(function(err){  //只要有任何promise执行失败，则立即终止promise的执行并进入catch，处理抛出的错误
  console.log('抛出错误，不执行后续promise'+err.message);
})
```

</br>

#### Jquery中的Ajax使用Promise
- 一般是通过success和error回调函数来处理请求结果

```js
$(function () {
  $('#btn').on('click', function () {
    $.ajax({
      url: './data.json',
      type: 'get',
      dataType: 'json',
      success: function (data) {
        console.log(data);
      }
    })
  })
})
```
- jquery提供了Promise的用法，可以通过.then()方法指定调用成功的函数

```js
$(function () {
  $('#btn').on('click', function () {
    $.ajax({
        url: './data.json',
        type: 'get',
        dataType: 'json'
      })
      .then(function (data) {
        console.log(data);
      })
  })
})
```