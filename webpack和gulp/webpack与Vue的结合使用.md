### webpack与Vue的结合使用

#### render方法渲染组件
- render方法只能渲染一个组件

```html
<div id="app">
    <login></login>
</div>

<script>
var login = {
  template: '<h1>登录组件</h1>'
}
var vm = new Vue({
  el: '#app',
  data: {},
  methods: {},
  render: function (createElements) { //createElements是一个方法，调用该方法可以把指定的模板渲染为html结构
    return createElements(login); //return的结果会替换页面el指定的容器，即#app元素被替换
  }
});
</script>
```

#### 在webpack中导入vue
- 在普通网页中使用vue的步骤
  + 用script标签引入vue.js
  + 创建id为app的div容器
  + 通过new Vue()创建一个vm实例
- 在webpack中使用vue，先安装`npm i vue -S`
  + 在main.js使用`import Vue form 'vue'`导入包的时候，先在项目根目录查找node_modules，再根据包名查找vue文件夹，在该文件夹下查找package.json，在文件内找到main属性
    - 包加载的入口文件，默认是vue.runtime.js，提供的vue构造函数功能不完整，使用完整版则需要在webpack.config.js中的module.exports添加resolve对象
  ```js
  module.exports = {
      //···
      resolve: {
        alias: {
          //配置当导入的包名是以vue结尾的时候，就使用该路径的包
          'vue$': 'vue/dist/vue.esm.js'// 用 webpack 1 时需用 'vue/dist/vue.common.js'
        }
      }
  }
  ```
  
  + 使用`import Vue from './node_modules/vue/dist/vue.js'`通过路径查找，导入完整的包

#### runtime-only下使用render函数
- 不完整版本`import Vue form 'vue'`即runtime-only，不能直接使用components渲染组件，先通过.vue文件创建组件

```html
<template>
  <div>这里是.vue文件内的登录模板</div>
</template>
<style>
</style>
<script>
</script>
```
- 默认webpack无法打包vue文件，先安装能识别.vue文件的loader `npm i vue-loader vue-template-compiler -D`
- 在配置文件webpack.config.js中配置` {test:/\.vue$/,use:'vue-loader'}`,然后在webpack.config.js中配置插件

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

- 在页面创建id为app的div元素，作为vue实例要控制的区域，然后在main.js中创建vue实例，导入模板文件.vue，并添加render方法

```js
import Vue from 'vue'
import login from './login.vue'

var vm = new Vue({
  el: '#app',
  data: {
    msg: 'vue实例中的msg'
  },
  render: c => c(login)  //返回的html会替换原来的#app元素
})
```

#### export default与export的使用
> ES6语法是用export default 与 export导出，用import导入；Node中使用 module.exports 与 exports导出，用`var 名称 = reqiure('模块标识符')`导入

- export default方式导出
  + 该方式向外暴露的成员，可以通过任意变量来接收 `import 变量名 from 'js文件路径'`
  + 在一个模块中，export default只允许向外暴露1次，否则会报错
  + 在一个模块中，可以同时使用export default 和export向外暴露成员

```js
var info = {
  name: 'hym',
  age: 18
}
export default info
```

- export 方式导出
  + 该方式导出的成员只能通过{导出的名称1，导出的名称2} 大括号的形式来接收，该形式叫 按需导出 ，可以按照需要导出和导入
  + export可以向外暴露多个成员，且不需要该成员的时候，则在import的时候不在{}中定义
  + 用export方式导出的成员必须严格按照导出的名称来导入
  + 导入时可以通过as重命名成员

```js
export var title = '这是导出的成员'
export var name = 'hym'
```

```js
//导入
import testInfo, { title as titlechange, name } from './js/testExport.js'

console.log(testInfo)
console.log(name)
console.log(titlechange)
```

#### 在webpack中使用vue-router
- 安装`npm i vue-router -S`,在mian.js中导入 `import vueRouter from 'vue-router'`
- 创建路由对象

```js
//mian.js
import Vue from 'vue'
import vueRouter from 'vue-router'
Vue.use(vueRouter)
import pageone from './view/firstpage.vue'
import pagetwo from './view/secondpage.vue'
var router = new vueRouter({
    routes:[
      {path:'/pageone',component:pageone},
      {path:'/pagetwo',component:pagetwo}
    ]
})

//在vue实例中添加router配置
var vm = new Vue({
  el: '#app',
  render: c => c(login),  //在webpack中如果想要通过Vue将一个组件放到页面中展示，要是用vm实例中的render函数
  router
})
```
- 在login组件中使用router-link与router-view

```html
<template>
  <div>
    <div>这里是.vue文件内的登录模板</div>
    <router-link to="/pageone">pageone</router-link>
    <router-link to="/pagetwo">pagetwo</router-link>
    <router-view></router-view>
  </div>
</template>
```

#### 结合webpack实现children子路由
- 创建两个子组件childrenone.vue与childrentwo.vue，在main.js路由配置中配置子路由

```js
import pageone from './view/firstpage.vue'
import pagetwo from './view/secondpage.vue'
import childrenone from './view/childrenone.vue'
import childrentwo from './view/childrentwo.vue'
var router = new vueRouter({
  routes: [
    {
      path: '/pageone',
      component: pageone,
      children: [
        { path: 'childrenone', component: childrenone },
        { path: 'childrentwo', component: childrentwo }
      ]
    },
    { path: '/pagetwo', component: pagetwo }
  ]
})
```
- 在组件页面内创建路由跳转

```html
<template>
  <div>
    <h1>这是第一个组件的页面page1</h1>
    <router-link to="/pageone/childrenone">display childrenone</router-link>
    <router-link to="/pageone/childrentwo">display childrentwo</router-link>
    <router-view></router-view>
  </div>
</template>
```

#### 组件中style标签lang属性与scoped属性
- 在组件中，默认style标签内设置的是全局的样式，即子组件内的style会影响到父组件的style，需要给style添加scoped属性设置为私有
- 普通的style标签只支持普通的样式，要使用less或者scss需要给style标签设置lang属性，指定使用的样式
- 如果style标签是在.vue组件中定义，都推荐为style开启scoped属性

```html
<style scoped lang="scss">
body {
  div {
    font-style: italic;
  }
}
</style>
```

#### 抽离路由模块
- 将组件导入和路由创建部分的代码放到main.js同级目录下的router.js中（注意引入vue-router）
- 在router.js中暴露创建的router对象`export default router`
- 在mian.js中导入router对象 `import router from './router.js'`