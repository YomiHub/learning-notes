### webpack之loader的使用

#### loader配置处理css样式表
> 直接在页面内通过link标签引入文件会引起二次请求，一般是在入口js文件通过import引入，但是不能识别该文件类型（webpack默认只能打包处理JS文件，如果想要处理非JS文件则需要安装loader加载器）

- 使用命令`npm i style-loader css-loader --save-dev`安装样式加载器
- 在入口JS文件main.js中引入css `import ('./css/index.css')`
- 在webpack.config.js文件的module.exports中添加module配置节点，在module对象上有一个rules属性，该属性对应值为数组类型，用于存放第三方文件的匹配和处理规则，数组成员对象包含test属性（正则匹配文件后缀）与use（该文件的loader）
- 在pakage.json的scripts中添加`"dev": "webpack-dev-server --open --port 3000 --contentBase src --hot"`,运行时直接使用命令`npm run dev`

```js
const path = require('path');
const webpack = require('webpack');
const htmlWebpackPluin = require('html-webpack-plugin'); //自动将打包的文件引入生成的html中

//使用Node中的模块操作，向外暴露了一个配置对象，直接运行命令webpack运行该文件
module.exports = {
  mode: 'development',  // webpack 使用相应环境
  entry: path.join(__dirname, './src/main.js'),   //指定webpack打包的文件
  output: {
    path: path.join(__dirname, './dist'),  //指定输出文件的目录
    filename: 'bundle.js'  //指定输出文件的名称
  },
  devServer: {
    open: true,
    port: 3000,
    contentBase: 'src',
    hot: true
  },
  plugins: [  //配置插件的节点
    new webpack.HotModuleReplacementPlugin(),
    new htmlWebpackPluin({  //创建一个在内存生成html页面的插件
      //指定模板页面，将来会根据指定的页面路径去生成内存的页面
      template: path.join(__dirname, './src/index.html'),
      filename: 'index.html'  //指定生成页面的名称
    })
  ],
  module: {
    rules: [
      { test: /\.css$/, use: ['style-loader', 'css-loader'] }  //配置处理.css文件的第三方loader
    ]
  }
}
```

</br>

#### webpack处理第三方文件
- 发现要处理的文件不是JS类型，就去配置文件中查找有没有对应第三方loader规则
- 如果找到对应规则，就会调用对应的loader处理这种文件类型
- 调用loader的时候，从后往前调用
- 到最后一个loader调用完毕会把处理的结果直接交给webpack进行打包合并，最终输出到bundle.js中去

</br>

#### 配置处理less文件的loader
- 安装less-loader的依赖`npm i less -D`
- 使用命令`npm i less-loader -D`安装less-loader
- 在main.js中引入less文件`import './css/index.less'`
- 在配置中添加less-loader

```js
  module: {
    rules: [
      { test: /\.less$/, use: ['style-loader', 'css-loader','less-loader'] }  //配置处理.less文件的第三方loader
    ]
  }
```

</br>

#### 配置处理scss文件的loader
- 安装sass-loader的依赖`npm i node-sass -D`，如果使用npm无法顺利安装，就切换到cnpm仓库地址`nrm use cnpm`（前提是已经安装nrm）
- 使用命令`npm i sass-loader -D`安装sass-loader
- 在main.js中引入scss文件`import './css/index.scss'`
- 在配置中添加sass-loader

```js
  module: {
    rules: [
      { test: /\.scss$/, use: ['style-loader', 'css-loader','sass-loader'] }  //配置处理.scss文件的第三方loader
    ]
  }
```

</br>

#### 使用webpack处理css中的路径
- 默认情况下webpack不能处理css文件中的url地址，无论是图片还是字体库 
- 需要安装url-loader及其内部依赖file-loader：`npm i url-loader file-loader --save-dev`
- 在webpack.config.js中配置处理url路径的loader（默认情况会将图片转成base64）

```js
  module: {
    rules: [
      { test: /\.(jpg|png|gif|bmp|jpeg)$/, use: 'url-loader' }
    ]
  }
```
- 通过limit参数指定转成base64的图片大小（以字节byte为单位，小于等于该值才转，大于不会转base64，而是以hash命名的图片显示），还可以通过name参数指定图片名称（注意直接使用原来名称，会导致不同目录同名称的图片被覆盖）

```js
  module:{
      rules:[
       { test:/\.(jpg|png|gif|bmp|jpeg)$/, use:'url-loader?limit=1234&name=[hash:8]-[name]' }  //8位hash值-原来的图片名称
      ]
  }
```
- webpack处理字体文件
  + 安装bootStrap `npm install bootstrap@3 -S`
  + 在main.js中引入字体文件（在node_modules中查找引入的文件）`import 'bootstrap/dist/css/bootstrap.css'`

```js
  module:{
      rules:[
       { test:/\.(ttf|eot|svg|woff|woff2)$/, use:'url-loader' }  //bootstrap.css中引入字体文件的处理
      ]
  }
```

</br>

#### 在webpack中使用babel处理高级JS语法
> webpack默认只能处理一部分ES6语法，一些更高级的ES6与ES7语法是需要通过loader转为低级语法后，webpack才可以处理打包到bundle.js中

- 通过babel可以帮我们将高级语法转为低级语法，在webpack安装babel相关的loader（两套包）
  + 第一套包([runtime]("https://segmentfault.com/a/1190000009065987"))： `npm i babel-core babel-loader@7 babel-plugin-transform-runtime -D`
  + 第二套包（[babel-preset-env]("https://segmentfault.com/p/1210000008466178")）： `npm i babel-preset-env babel-preset-stage-0 -D`
- 打开webpack的配置文件，在modules的rules数组中新增匹配规则`{test:/\.js$/,use:'babel-loader',exclude:/node_modules/}`
  + 如果不排除node_modules，则babel会把node_modules中所有第三方js文件打包编译，非常消耗CPU，而且打包速度很慢
  + 就算将node_modules中的js都打包完毕，项目也无法正常运行
- 在项目根目录（这里是src）下新建.babelrc的配置文件，该文件是Json格式，所以不能写注释且字符串必须用双引号
  + 在配置文件中添加如下配置（可以将）presets翻译为语法的意思

```json
{
    "presets":["env","stage-0"],
    "plugins":["transform-runtime"]
}
```
注：以上为使用7.x的babel-loader，如果出现Error: Cannot find module '@babel/core’或者想要使用8.x版本则可参考问题篇[problem.md]("https://github.com/YomiHub/learning-notes/blob/master/webpack/problem.md ")