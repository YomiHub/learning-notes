### ES6开发环境

> 大部分浏览器还不支持es6语法，需要搭建es6的开发环境将es6编译成es5

#### 使用Bable将ES6编译成ES5
- 创建工程目录为Test，命令`npm init -y`生成的默认的package.json(下载代码后就只需要执行 `npm install` 后就会自动安装项目依赖的包)
- 在项目工程下创建index文件夹用于存放开发中编写的ES6代码以及Bable编译生成的ES5
- 进入到项目目录，使用命令安装Babel(需要先安装好node环境) 
```
$ npm install --save-dev @babel/core
```
- 在项目根目录创建配置文件.babelrc，用于设置转码规则与插件，首先需要安装规则集
```
# 最新转码规则
$ npm install --save-dev @babel/preset-env

# react 转码规则
$ npm install --save-dev @babel/preset-react
```
- 将规则加入到.babelrc中
```
  {
    "presets": [
      "@babel/env",
      "@babel/preset-react"
    ],
    "plugins": []
  }
```


#### 开发环境
>  @babel/register模块

- 模块改写require命令，为它加上一个钩子。此后，每当使用require加载.js、.jsx、.es和.es6后缀名的文件，就先用 Babel 转码
```
$ npm install --save-dev @babel/register
```

- 使用时需要在文件加载@babel/register
```
// index.js
require('@babel/register');
require('./es6.js');
```
- 配置后就不需要手动对index.js转码，直接运行`node index.js`



> 浏览器环境下 @babel/standalone

- 浏览器实时转码会对性能有影响
```html
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">
// Your ES6 code
</script>
```


#### 生产环境
> 命令行转码工具 @babel/cli

- 安装转码工具
```
$ npm install --save-dev @babel/cli
```
- 使用工具转码得到转码后的文件
```
# 转码结果输出到标准输出
$ npx babel example.js

# 转码结果写入一个文件
# --out-file 或 -o 参数指定输出文件
$ npx babel example.js --out-file compiled.js
# 或者
$ npx babel example.js -o compiled.js

# 整个目录转码
# --out-dir 或 -d 参数指定输出目录
$ npx babel src --out-dir lib
# 或者
$ npx babel src -d lib

# -s 参数生成source map文件
$ npx babel src -d lib -s
```

> 在线编译器
- Babel 提供一个REPL [在线编译器](https://babeljs.io/repl/)，可以在线将 ES6 代码转为 ES5 代码。转换后的代码，可以直接作为 ES5 代码插入网页运行

</br>

#### 注意
> Babel 默认只转换新的 JavaScript 句法（syntax），而不转换新的 API，比如Iterator、Generator、Set、Map、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。

- 举例来说，ES6 在Array对象上新增了Array.from方法。Babel 就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill，为当前环境提供一个垫片
```
$ npm install --save-dev @babel/polyfill
```

- 在es6脚本头部引入
```
import '@babel/polyfill';
// 或者
require('@babel/polyfill');
```