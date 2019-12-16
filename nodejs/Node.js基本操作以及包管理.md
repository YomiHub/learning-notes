### Node.js基本操作以及包管理

> Node.js 是一种建立在Google Chrome’s v8 engine上的 non-blocking (非阻塞）, event-driven （基于事件的） I/O平台. 
> </br>
> </br>Node.js平台使用的开发语言是JavaScript，平台提供了操作系统低层的API，方便做服务器端编程，具体包括文件操作、进程操作、通信操作等系统模块

#### win+r 窗口命令
- notepad 打开记事本
- mspaint 打开画图
- calc 打开计算机
- write 写字板
- sysdm.cpl 打开环境变量设置窗口


#### cmd窗口命令
- md 创建目录
- rmdir(rd) 删除目录，目录内没有文档。
- echo on > a.txt 创建空文件
- echo 内容 > a.txt  覆盖文件内容，两个>>表示追加内容
- del 删除文件
- rm 文件名 删除文件
- cat 文件名 查看文件内容
- cat > 文件名 覆盖文件内容,两个>>表示追加，Ctrl+C退出
- rd \s\q 删除非空目录

#### 常用的命令
- 查看已经安装的版本 nvm list
- 切换node版本：nvm use 12.10.0
- 查看当前使用的版本：node -v
- 卸载指定版本nvm uninstall 指定版本号
- 注：npm下载不成功时，自己去下一个 然后放到D:nvmv8.12.0node_modules下
同时将我安装之前的版本npm npm.cmd 这两个文件复制过去

#### 关于node
- REPL: read-eval-print-loop 读取代码-执行-打印结果-循环
- 执行命令node进入repl，命令.exit退出repl
- [Globals](https://nodejs.org/dist/latest-v12.x/docs/api/globals.html)
    - __filename:包含文件名的全路径
    - __dirname：不包含文件名的当前文件路径
    - setTimeout：定时
    - process.argv：是一个数组，默认前两项是Node.js环境的路径、当前执行文件的全路径（作用是接收命令行传入的参数）
    - process.arch：打印操作系统架构x64、x86

#### 模块化
- 前端标准模块化规范
    - AMD - requirejs
    - CMD - seajs
- 服务端的模块化规范
    - CommonJS - Node.js
- 模块化的规则
     1. 定义模块：一个js文件就是一个模块，模块内部的成员和方法相互独立
     2. 模块成员的导出与引入（还可以借助global导入导出，比较不常用）
```js
//a.js
function sum(a, b) {
  console.log(a + b);
}
exports.sum = sum;
//方法二导出
module.exports = sum;

//b.js
var module = require("./a.js");
module.sum(12, 13);
//方法二使用
module(12,13);
```

- 已经加载的模块会有缓存（模块后缀一般是.js、.json、.node）
- 模块加载规则
    - 模块查找 不加扩展名的时候会按照如下后缀顺序进行查找 .js .json .node
- 模块分类
    + 自定义模块
    + [系统核心模块](https://nodejs.org/dist/latest-v12.x/docs/api/)
      * fs 文件操作
      * http 网络操作
      * path 路径操作
      * querystring 查询参数解析
      * url url解析

----
#### ES6常用语法
- 变量声明let与const
  + let声明变量不存在预解析，只能先声明再使用
  + let在同作用域内不允许重复声明
- 变量的解构赋值
  + 数组解构赋值 `let [a=0,b,c] = [1,2,3]`
  + 对象解构赋值 `let{a:b="默认值",c} = {c:'2',a:'1'}` 其中b是a的别名，访问时只能访问别名b原来的名字a已经无效
  + 字符串解构赋值 `let [a,b,c,d,e] = "hello"`
- 字符串扩展
  + includes("匹配的字符串","开始匹配的位置") 判断字符串中是否包含某个字符串
  + startsWith()
  + endsWith()
  + 模板字符串 `var str = `<div><span>${obj.username}<span></div>` `模板的内容可以有格式，${可以是算式或者函数调用等}
- 函数扩展
  + 参数默认值   `function sum(a=1,b=2)`
  + 参数结构赋值 `function sum({a=1,b=2})   sum({a=3,b=4})`
  + rest参数  `function sum(a,...param)` 数组param接收剩余参数
  + ...扩展运算符   `function sum(a,b,c) sum(...[1,2,3])`
  + 箭头函数    `let foo = () => console.log("hello")` 函数中的this取决于函数的定义、不能使用arguments获取实参列表应该用rest参数即...param、箭头函数不能使用new
- 类与继承
```js
//es6
class Animal{
    static showInfo(){
        console.log("只能通过类名Animal.showInfo()调用静态方法")
    }
    constructor(name){
        this.name=name;
    }
    showName(){
        console.log(this.name);
    }
}

//静态方法也可以继承，通过类名调用
class Cat extends Animal{
    constructor(name,color){
         super(name);  //调用父类
         this.color = color;
    }
}

```

----
#### [Buffer基本操作](https://nodejs.org/dist/latest-v12.x/docs/api/buffer.html)
> Buffer对象是Node处理二进制数据的一个接口。它是Node原生提供的全局对象，可以直接使用，不需要require(‘buffer’)。

- 实例化
  + Buffer.from(array)  //转16进制,可以指定编码Buffer.from([0x62,0x75,0x66],'base64')=>buf,默认编码是utf8
  + Buffer.from(string)  //转16进制
  + Buffer.alloc(size)   //初始化空Buffer

- 功能方法
  + Buffer.isEncoding() 判断是否支持该编码,Buffer.isEncoding("gtk")
    - 支持：ascii、utf8、utf16le、ucs2、base64、latin1、binary、hex
  + Buffer.isBuffer() 判断是否为Buffer
  + Buffer.byteLength() 返回指定编码的字节长度，默认utf8（使用utf8时三个字节表示一个中文字符）
  + Buffer.concat() 将一组Buffer对象合并为一个Buffer对象`Buffer.concat([buf1,buf2])`

- 实例方法
  + write() 向buffer对象中写入内容（使用aloc初始化之后可以write写入，参数分别是写入的字符、开始写入的位置、写入字符的长度）
  + slice() 截取新的buffer对象，默认截取所有，返回新的buffer，参数分别为开始截取位置、结束截取的位置
  + toString() 把buf对象转成字符串
  + toJSON() 把buf对象转成json形式的字符串，隐式调用，只需要`var json = JSON.stringfy(buf);`内部自动调用该方法

----
#### [路径操作](https://nodejs.org/dist/latest-v12.x/docs/api/path.html)
> 使用的时候需要const path = require('path');引入

- 基本操作API(linux下是/windowwei\且带有盘符)
    + `path.basename('/test/testhtml/test.html');`返回的是test.html，当设置第二个参数为.html的时候会返回文件名test
    + `path.dirname('/test/testhtml/test.html');`返回路径不包含文件名
    + `path.extname('index.html')`返回扩展名.html
    + `path.format()`  将obj转换为string
    + `path.parse(__filename)`将string转换为obj，obj包括root、dir（文件全路径）、base（带扩展的文件名）、ext、name
    + `path.isAbsolute('C:/foo/test')` 在window判断是否为盘符开始，在linux中判断是否以/根路径开始
    + `path.join()`拼接路径（会解析特殊字符，比如..表示上层目录）
    + `path.normalize()`将多余的/或者特殊字符..规范化为规范路径表示
    + `path.relative()`返回相对第一个参数而言，第二个参数的相对路径
    + `path.resolve()`解析相当于cd操作，将所有参数连成完整的路径
    + 属性`path.delimiter`表示路径分隔符，windows用\，linux用/
    + 属性`path.sep`表示环境变量分隔符，windows用分号；linux使用冒号：

----
#### [文件操作](https://nodejs.org/dist/latest-v12.x/docs/api/fs.html)
> 异步I/O
- 在浏览器中也存在异步操作
    + 定时任务
    + 事件处理
    + Ajax回调处理
- 在Node中进入事件队列（异步执行）的任务有（基于回调函数的编码风格）
    + 文件读写操作
    + 网络请求响应处理

> 文件信息获取——使用需要const fs = reqiure('fs')引入
- 判断文件是否存在，不再在则在err保存错误信息`fs.stat('./data.txt',(err,stat)=>{})`一般回调函数的第一个参数是错误的对象，如果err为null，表示没有错误否则表示报错.对应的同步方法 `fs.statSync('./data.txt')`返回stat对象
- 常用方法：`stat.isFile()`判断是否为文件、`stat.isDirectory()`判断是否为目录
- 常用属性：
    + stat.atime 访问时间access time
    + stat.ctime  文件状态信息发生变化的时间，比如文件的权限change time
    + stat.mtime 文件数据发生变化的时间 modefied time
    + stat.birthtime 文件创建的时间

> 读文件操作
- `fs.readFile(strpath,(err,data)=>{if(err) return; console.log(data.toString())})`没有第二个参数返回的data是buffer通过toString()转换为字符串，当设置第二个参数为编码格式，则回调获取的data是字符串，同样对应同步方法var data = fs.readFileSync(strpath,'utf8')

> 写文件操作
- `fs.writeFile(strpath,'hello world',utf8',(err)=>{if(!err){console.log("写入成功")}})`

> 大文件操作（流式操作）
- `fs.createReadStream(path[,options])、fs.createWriteStream(path,[,options])`对应的同步方法是`var buf = Buffer.from("Tom and Jerry"); fs.writeFileSync(strpath,buf) //写入成功是没有提示的`

```js
const path = require("path");
const fs = require("fs");

let sPath = path.join(__dirname, 'test.js');
let dPath = path.join('C:\\Users\\eit', 'test.js');

let readStream = fs.createReadStream(sPath);
let writeStream = fs.createWriteStream(dPath);

//没有dom操作，通过文件读取事件处理
let num = 1;
readStream.on('data', (chunk) => {
  num++;
  writeStream.write(chunk);
})

readStream.on('end', () => {
  console.log("文件处理完成,处理次数" + num)
})
```

- 管道`pipe()`将输入流和输出流连接到一块（从磁盘写入内存为输入，从内存写入磁盘为输出）

```js
//处理大文件的另一种简单处理
readStream.pipe(writeStream);  //fs.createReadStream(sPath).pipe(fs.createWriteStream(dPath))
```

> 目录操作
- 创建目录

```js
const path = require("path");
const fs = require("fs");
fs.mkdir(path.join(__dirname, 'test'), (err) => {
  console.log(err);  //null表示创建成功
})

//对应同步的操作 fs.mkdirSync(path.join(__dirname,'test2'));

```

- 读取目录

```js
const path = require("path");
const fs = require("fs");
fs.readdir(__dirname, (err, files) => {
  if (err) return;
  console.log(files);  //当前目录下所有文件及目录组成的数组
  files.forEach((item, index) => {
    fs.stat(path.join(__dirname, item), (err, stat) => {
      if (stat.isFile()) {
        console.log(item, "文件类型")
      } else if (stat.isDirectory()) {
        console.log(item, "目录类型")
      }
    })
  })
})

//同步方法的操作
let files = fs.readdirSync(__dirname);
files.forEach((item,index)=>{});//处理同上
```

- 删除目录

```js
const path = require("path");
const fs = require("fs");

fs.rmdir(path.join(__dirname, 'test'), (err) => {
  console.log(err);  //打印null表示成功删除
})

//对应的同步方法 fs.rmdirSync(path.join(__dirname, 'test'));  
```

-----
### 包
- 多个模块可以形成包，不过要满足特定的规则才能形成规范的包（一个特定的模块完成一个特定子功能）

#### NPM（ode.js package management）
> 全球最大的模块生态系统，里面所有的模块都是开源免费的；也是Node.js的包管理工具。
- [npm官网](https://www.npmjs.com/):托管了所有开源的npm包

</br>

#### npm包安装的方式（可以指定安装包的版本）

- 初始化项目：在项目目录中执行`npm init -y`(参数-y表示使用默认设置)，生成package.json(执行包的命令：`node .`就是执行main指向的文件；另外`npm run test`可以执行test中的指令`node index.js`)
- 全局安装 -g（安装的包都在node环境的node_modules里面，通过cmd窗口直接安装）`npm install -g 包的名称@版本号`；对应卸载全局的安装包`npm uninstall -g 包的名称`；对应更新包（默认更新到最新版，也可以直接install不用update）的命令`npm update -g 包的名称@版本号`（全局安装一般用于命令行工具，直接可以在命令行执行）
- 本地安装 `npm install 包的名称`其余操作同上，不需要加-g （一般用于具体的功能实现，安装的包在项目目录下的node_modules里面，使用时通过require('包名')引入）

```
//常用的包
npm install -g es-checker //用于检测node环境对es6的支持程度，用命令es-checker检测 https://github.com/ruanyf/es-checker
npm install -g i5ting_toc //用于将markDown转成网页https://github.com/i5ting/tocmd.npm（进入mark所在文件夹执行i5ting_toc -f 文件名.md -o）
npm install art-template  //前后端通用的模板引擎https://github.com/aui/art-template 对应中文文档https://aui.github.io/art-template/zh-cn/index.html
```

</br>

#### npm常用的命令参数
- 生产环境添加依赖--save：`npm install art-template --save` 会在package.json中添加该包的依赖dependencies
- 开发环境添加的依赖 --save-dev：`npm install art-template --save-dev`会在package.json中添加开发环境下的包依赖devDependencies，比如一些babel编译功能的插件、webpack打包插件
- 安装生产环境的依赖node-moudules：在项目目录执行`npm install --production`；都安装的时候用命令`npm install`

</br>

#### 解决npm安装包被墙的问题
- --registry
  + `npm config set registry https://registry.npm.taobao.org `可以改变默认下载地址，依旧使用npm，达到不安装cnpm就使用淘宝镜像的目的；验证用命令`npm config get registry` 或 `npm info underscore`
- cnpm（推荐使用）
  + 淘宝NPM镜像,与官方NPM的同步频率目前为10分钟一次 
  + 官网: http://npm.taobao.org/ 
  + `npm install -g cnpm --registry=https://registry.npm.taobao.org`测试是否成功安装:`cnpm -v `
  + 使用cnpm安装包: `cnpm install 包名`
- nrm
  + 作用：修改镜像源 
  + 项目地址：https://www.npmjs.com/package/nrm 
  + 安装：`npm install -g nrm`
  + 添加registry地址
    + nrm add npm http://registry.npmjs.org
    + nrm add taobao https://registry.npm.taobao.org
  + 切换npm registry地址
    + nrm use taobao
    + nrm use npm
  + nrm ls即nrm list，查看所有可用的镜像

</br>

#### yarn 工具基本使用  `npm install -g yarn`
> 性能上比npm更加优越，提示更加友好

- 对应初始化：`yarn init`
- 对应安装：`yarn add 包名` 默认--save保存到依赖
- 安装开发环境包：`yarn add 包名 --dev`
- 对应的全局安装：`yarn global add 包名`
- 移除包：`yarn remove 包名`
- 更新包：`yarn upgrade 包名`
- 设置下载镜像的地址：`yarn config set registry url`与npm方式一样
- 项目安装所有包：`yarn install`
- 项目安装生产环境的包: `yarn run`

----
### 自定义包
#### 包的规范
- package.json必须在包的顶层目录下
- 二进制文件应该在bin目录下
- JavaScript代码应该在lib目录下
- 文档应该在doc目录下
- 单元测试应该在test目录下


#### package.json字段分析
- name：包的名称，必须是唯一的，由小写英文字母、数字和下划线组成，不能包含空格
- description：包的简要说明
- version：符合语义化版本识别规范的版本字符串
- keywords：关键字数组，通常用于搜索
- maintainers：维护者数组，每个元素要包含name、email（可选）、web（可选）字段
- contributors：贡献者数组，格式与maintainers相同。包的作者应该是贡献者数组的第一- 个元素
- bugs：提交bug的地址，可以是网站或者电子邮件地址
- licenses：许可证数组，每个元素要包含type（许可证名称）和url（链接到许可证文本的- 地址）字段
- repositories：仓库托管地址数组，每个元素要包含type（仓库类型，如git）、url（仓- 库的地址）和path（相对于仓库的路径，可选）字段
- dependencies：生产环境包的依赖，一个关联数组，由包的名称和版本号组成
- devDependencies：开发环境包的依赖，一个关联数组，由包的名称和版本号组成