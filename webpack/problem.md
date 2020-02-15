#### 安装babel包，Error: Cannot find module '@babel/core’  问题

> 问题产生的原因: babel-loader和babel-core版本不对应所产生的

- babel-loader 8.x对应babel-core 7.x（这些@开头的包，在实用npm安装时，包名必须用引号引住）
- babel-loader 7.x对应babel-core 6.x

> 参考解决方案一[原文链接](https://www.cnblogs.com/QianDingwei/p/10800795.html)

- 如果已经安装了babel-loader 7.x，希望使用babel-loader 8.x
1、 卸载旧的
`npm un babel-core`
`npm un babel-preset-env`
`npm un babel-preset-stage-0`
`npm un babel-plugin-transform-runtime`

2、 安装新的babel-loader 8.x
- babel-loader：加载器
- @babel/core：babel核心包,babel-loader的核心依赖
- @babel/preset-env：ES语法分析包
- @babel/runtime和@babel/plugin-transform-runtime：babel 编译时只转换语法，几乎可以编译所有时新的 JavaScript 语法，但并不会转化BOM（浏览器）里面不兼容的API。比如 Promise,Set,Symbol,Array.from,async 等等的一些API。这2个包就是来搞定这些api的。
- @babel/plugin-proposal-class-properties：用来解析类的属性的。
- 修改.babelrc文件

```json
{
    "presets": ["@babel/preset-env"],
    "plugins": [
        "@babel/plugin-transform-runtime",
        "@babel/plugin-proposal-class-properties"
    ]
}
```

- 安装使用babel-loader@7.x
  + `npm i babel-core babel-loader@7 babel-plugin-transform-runtime -D`
  + `npm i babel-preset-env babel-preset-stage-0 -D`
  + webpack.config.js配置文件中，rules节点的配置写法　　`{ test: /\.js$/, use: 'babel-loader',exclude:/node_modules/}`

```
{
  "presets": ["env", "stage-0"],
  "plugins": [
    "transform-runtime",
  ]
}
```

- 参考解决方案二[原文链接](https://www.jianshu.com/p/74cb6203c39f)

  1、 卸载旧的babel-core 
  `npm un babel-core`

  2、 安装新的babel-core 
  `npm i -D '@babel/core'`

  3、 卸载旧的babel-preset 
  `npm un babel-preset-env npm un babel-preset-stage-0`

  4、 安装新的babel-preset 
  `npm i '@babel/preset-react' npm i '@babel/preset-env' npm i babel-preset-mobx`

  5、 卸载旧的babel-plugin 
  `npm un babel-plugin-transform-runtime`

  6、 安装新的babel-plugin 
  `npm install --save-dev '@babel/plugin-proposal-object-rest-spread' `
  `npm install --save-dev '@babel/plugin-transform-runtime' `
  `npm install --save '@babel/runtime'`

  7、 修改.babelrc文件

```
{
    "presets": ["@babel/preset-env", "@babel/preset-react", "mobx"],
    "plugins": [
        "@babel/plugin-proposal-object-rest-spread",
        "@babel/plugin-transform-runtime"
    ]
}
```
</br>

#### webpack打包vue项目，Error: listen EADDRINUSE: address already in use 127.0.0.1:3000
- 打开cmd(win+R快捷键)
- 运行netstat -ano，找到报错信息提示的端口号那一行，记住最后那个数字
- 接下来运行tskill “最后那个数字”，所以这里我运行的是tskill 20212

</br>


#### Vue-loader在15.*之后的版本使用 webpack vue-loader was used without the corresponding plugin. Make sure to include VueLoaderPlugin  报错参考官方文档[链接]("https://vue-loader.vuejs.org/migrating.html#a-plugin-is-now-required")
```js
//在webpack.config.js中加入
const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
  // ...
  plugins: [
    new VueLoaderPlugin()
  ]
}
```

