## 开发环境和目录
- `npm init -y`
- `npm install -g typescript`
- `tsc --init`创建配置文件tsconfig.json
- 创建src存放TS源文件index.ts，使用命令编译
  - `let hello : string = "hello world"`
  - 编译命令：`tsc ./src/index.ts`，执行后自动在同目录下生成index.js

## 手动配置 tsconfig.json
- 构建模式是手动命令编译时才会去编译,监视模式会自动编译
- 打开vscode的 "终端" Terminal，选择`tsc: watch - tsconfig.json`配置监视模式

```json
{
  "compilerOptions": {
   "target": "es5",    //一般设置为 ES5 或者 ES2016/2017
   "noImplicitAny": false,    //
   "module": "amd",
   "removeComments": false,
   "sourceMap": false,
   "outDir": "build"  //你要生成js的目录,可自由命名
  }
}
```

## 使用webpack构建工具配置
- `npm install webpack webpack-cli webpack-dev-server -D`
- 安装ts `npm i ts-loader typescript -D`
- 自动生成html页面 `npm i html-webpack-plugin -D`
- 创建build目录用于存放webpack配置文件
- 编写`公共配置：webpack.base.config.js、配置文件入口：webpack.config.js、开发环境：webpack.dev.config.js、生产环境：webpack.pro.config.js`

```js
//webpack.base.config.js
module.exports = {
  entry: './src/index.ts',
  output: {
    filename: 'app.js'
  },
  resolve:{
    extensions:['.js','.ts','.tsx']
  },
  module:{
    rules:[
      {
        test:/\.tsx?$/i,
        use:[{
          loader:'ts-loader'
        }],
        exclude:/node_modules/
      }
    ]
  },
  plugins:[
    new HtmlWebpackPlugin({ //创建一个在内存生成html页面的插件
      template:'./src/tpl/index.html',//指定模板页面，将来会根据指定的页面路径去生成内存的页面
      filename:'index.html'  //指定生成页面的名称
    })
  ]
};
```
- 开发环境配置devtool
  - eval：可以设断点调试，不显示列信息，每个js模块代码用eval()执行，在生成的每个模块代码尾部加上注释
  - source-map：可以设断点调试，不显示列信息，生成相应的.Map文件，块代码并没有被eval()包裹，此种模式并没有将调试信息放入打包后的代码中
  - eval-source-map：不能设断点调试，不显示列信息，可以用手动加入debugger调试;参考第一种eval模式，跟eval不一样的是其用base64存储map信息，不会生成.Map文件
  - cheap-source-map：可以设断点调试，不显示列信息，不包含 loader 的 sourcemap
  - cheap-module-source-map：不包含列信息，同时 loader 的 sourcemap 也被简化为只包含对应行的。最终的 sourcemap 只有一份
  - cheap-module-eval-source-map：可以设断点调试，不显示列信息，并在合并后的代码尾部以base64加入map信息
  - hidden-source-map：跟source-map 一样，只不过不在打包后的文件中加入注释，断点要在打包好的js中加

```js
//开发环境webpack.dev.config.js
module.exports={
  //此选项控制是否生成，以及如何生成 source map。
  //cheap忽略文件的列信息
  devtool:'eval-cheap-module-source-map' //cheap-module-eval-source-map 绝大多数情况下都会是最好的选择
}
```

- 生产环境配置
  - 打包的时候清空dist目录下无用的缓存 `npm i clean-webpack-plugin -D`

```js
//webpack.pro.config.js
const {CleanWebpackPlugin} = require('clean-webpack-plugin')

module.exports = {
  plugins:[
    new CleanWebpackPlugin()
  ]
}
```

- 配置文件入口
  - 用于合并配置的插件 `npm i webpack-merge -D`

```js
//webpack.config.js
const { merge }= require('webpack-merge');
const baseConfig = require('./webpack.base.config');
const devConfig = require('./webpack.dev.config');
const proConfig = require('./webpack.pro.config');

let config = process.NODE_ENV === 'development'?devConfig:proConfig;

module.exports = merge(baseConfig,config);
```

## 修改npm执行命令
- 在index.ts中将变量插入到页面中`document.querySelectorAll('.app')[0].innerHTML = hello;`
- 修改package.json的scripts属性，执行`npm start`运行项目，`npm run build`进行打包

```
{
   "start": "webpack-dev-server --mode=development --config ./build/webpack.config.js",
   "build":"webpack --mode=production --config ./build/webpack.config.js"
}
```