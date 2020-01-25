### problem

#### Error: Cannot find module '@babel/core’问题

> 问题产生的原因

babel-loader和babel-core版本不对应所产生的，

- babel-loader 8.x对应babel-core 7.x
- babel-loader 7.x对应babel-core 6.x

> 如何解决

1、 卸载旧的babel-core
`npm un babel-core`
2、 安装新的babel-core
`npm i -D @babel/core`
3、 卸载旧的babel-preset
`npm un babel-preset-env`
`npm un babel-preset-stage-0`
4、 安装新的babel-preset
`npm i @babel/preset-react`
`npm i @babel/preset-env`
`npm i babel-preset-mobx`
5、 卸载旧的babel-plugin
`npm un babel-plugin-transform-runtime`
6、 安装新的babel-plugin
`npm install --save-dev @babel/plugin-proposal-object-rest-spread`
`npm install --save-dev @babel/plugin-transform-runtime`
`npm install --save @babel/runtime`
7、 修改.babelrc文件

```json
{
    "presets": ["@babel/preset-env", "@babel/preset-react", "mobx"],
    "plugins": [
        "@babel/plugin-proposal-object-rest-spread",
        "@babel/plugin-transform-runtime"
    ]
}
```

原文链接：https://www.jianshu.com/p/74cb6203c39f