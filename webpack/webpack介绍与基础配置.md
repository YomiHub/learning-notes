### webpack介绍与基础配置

#### nrm的安装使用（切换镜像地址）

> 作用：提供了一些最常用的NPM包镜像地址（cnpm、taobao等），能够让我们快速的切换安装包时候的服务器地址；但使用的时候装包工具仍然是npm（知识下载源地址改变了）
- 运行`npm i nrm -g`全局安装`nrm`包；
- 使用`nrm ls`查看当前所有可用的镜像源地址以及当前所使用的镜像源地址；
- 使用`nrm use npm`或`nrm use taobao`切换不同的镜像源地址；

</br>

#### webpack介绍
> 页面中常使用到很多静态资源，例如js、css、images、字体文件、模板文件等，这将会让加载速度变慢，因为要发起很多的二次请求，而且还会带来错综复杂的依赖关系
- 解决上述问题：以合并、压缩、精灵图、图片的Base64编码，加快加载速度；可以使用requireJS、也可以使用webpack可以解决各个包之间的复杂依赖关系；
- webpack：前端的一个项目构建工具，它是基于 Node.js 开发出来的一个前端工具
- 解决上述两个问题的方案：
  + 使用Gulp（基于task任务）
  + 使用Webpack（基于整个项目进行构建）
    - 借助于webpack这个前端自动化构建工具，可以完美实现资源的合并、打包、压缩、混淆等诸多功能
    - [webpack官网](http://webpack.github.io/)

</br>

#### webpack安装
> webpack 3中，webpack本身和它的CLI以前都是在同一个包中，但在第4版中，他们已经将两者分开
- 方式一：`npm i webpack -g`全局安装webpack，以及CLI`npm install webpack-cli -g `这样就可以在全局使用webpack的命令
- 方式二：在项目根目录运行`npm i webpack --save-dev`局部安装到项目依赖中，以及`npm i webpack-cli --save-dev`

</br>

#### webpack最基本使用
> 新建项目webpackDemo，在项目目录下新建源代码目录'src'和存放发布产品的'dist'目录

- 在src目录下，一般基本的会有css、js、images文件夹，以及js入口文件和首页html文件
- 初始化项目`npm init -y`,安装jquery`npm i jquery -s`即`npm i jquery --save`
- webpack4引入了模式,有开发模式,生产模式,无这三个状态，所以可以在package.json.添加上"dev"和"build"

```json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack --mode development",
    "bulid": "webapck --mode production"
  },
```

- 在js入口文件main.js导入jquery

```js
//js入口文件
import $ form 'jquery'  //es6中导入模块的方式，直接运行浏览器无法解析（使用wenbpack解决）

$(function () {
  $('li:odd').css('backgroundColor', 'blue')
  $('li:even').css('backgroundColor', function () {
    return '#' + 'ccc'
  })
})
```

- webpack处理main.js文件（将高级浏览器不能识别的es6语法处理为可识别） `webpack ./src/main.js -o ./dist/bundle.js --mode development` 
- 将webpack处理之后的bundel.js文件引入首页index.html `<script src="../dist/bundle.js"></script>`

</br>

#### 基本配置文件的使用'webpack.config.js'
- 在项目的根目录下创建文件webpack.config.js，配置入口和出口
- 在项目目录下运行命令`webpack`进行打包
  + 查询，没有再命令中指定入口与出口
  + 在项目根目录查找文件webpack.config.js
  + 解析配置文件，得到配置对象
  + 得到配置对象指定的入口和出口，进行打包构建

```js
//webpack.config.js
const path = require('path');

//使用Node中的模块操作，向外暴露了一个配置对象，直接运行命令webpack运行该文件
module.exports = {
  mode: 'development',  // webpack 使用相应环境
  entry: path.join(__dirname, './src/main.js'),   //指定webpack打包的文件
  output: {
    path: path.join(__dirname, './dist'),  //指定输出文件的目录
    filename: 'bundle.js'  //指定输出文件的名称
  }
}
```

</br>

#### webpack-dev-server的基本使用
> 使用webpack-dev-server可以实现自动打包编译的功能（就不需要每次修改后都运行配置文件）

- 因为该工具依赖于webpack，且在全局安装的无效，所以需要在本地再安装一次`npm i webpack webpack-cli --save-dev`
- 运行`npm i webpack-dev-server -D` 安装这个项目到本地开发依赖中
- 安装完毕之后，该工具的使用与webpack命令的用法一样
- 因为是在本地安装的，所以不能直接在项目目录下使用命令，需要将执行的 命令配置到package.json的scripts中

```json
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server"
  },
```
- 运行命令 `npm run dev`（快捷键Ctrl+c退出），此时工具会将项目默认托管到 http://localhost:8080/ ，只要访问该路径就可以看到项目文件，点击src进入index.html
- webpack-dev-server 打包生成的bundle.js并没有存放到实际的磁盘，而是直接托管到电脑内存，所以在项目根目录无法找到bundle.js，但可以通过虚拟根路径访问

```html
<!--根据打包提示，index.html引入的实时编译的bundle.js为根目录-->
<script src="/bundle.js"></script>
```

- 修改main.js之后只需要保存，就会自动编译执行，无需刷新页面

</br>

#### webpack-dev-server 常用的命令
- `webpack-dev-server --open`  表示保存后直接打开浏览器（package.json设置如下方）
- `webpack-dev-server --open --port 3000 `  设置挂载本地服务的端口号为3000，保存后直接跳转localhost:3000/
- `webpack-dev-server --open --port 3000 --contentBase src` 打开浏览器时，默认打开的目录为src
- `webpack-dev-server --open --port 3000 --contentBase src --hot` 热加载，无需重新编译整个js文件，而是编译了修改过的代码；另外无需刷新页面，资源自动更新

```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --open --port 3000 --contentBase src --hot"
  },
```

- 实现上述配置效果的还有另一种方式（比较麻烦，相对不推荐）
  + 打开webpack.config.js，module.exports中添加`devServer`配置对象，包括属性 open:true、port:3000、contentBase:'src'、hot:true
  + 通过该方式启动热更新还需要引入webpack包`const webpack = require('webpack');`
  + 然后在module.exports添加pugins数组，用于配置插件节点`new webpack.HotModuleReplacementPlugin()`
  + package.json的scripts标签中配置`"dev": "webpack-dev-server"`，执行命令`npm run dev`

```js
//webpack.config.js
  devServer: {
    open: true,
    port: 3000,
    contentBase: 'src',
    hot: true
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin()
  ]
```


</br>

#### 使用`html-webpack-plugin`插件
> 作用主要有两个：自动在内存中根据指定页面生成HTMl、自动将打包好的bundle.js追加到内存的页面

- 运行`npm i html-webpack-plugin --save-dev`安装到开发依赖
- 修改`webpack.config.js`配置，引入插件`const htmlWebpackPlugin = require('html-webpack-plugin');`
- 在plugins配置中添加该插件
- 删除index.html中文件的引用`  <!--   <script src="/bundle.js"></script> -->`，该插件会自动创建标签引入文件
- 执行命令`npm run dev`

```js
//webpack.config.js
 plugins: [  //配置插件的节点
    new webpack.HotModuleReplacementPlugin(),
    new htmlWebpackPluin({  //创建一个在内存生成html页面的插件
      //指定模板页面，将来会根据指定的页面路径去生成内存的页面
      template:path.join(__dirname,'./src/index.html'),
      filename:'index.html'  //指定生成页面的名称
    })
  ]
```

