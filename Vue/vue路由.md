#### 关于路由
- 后端路由：基本所有的超链接都是URL地址，所有的URL地址都对应服务器上对应的资源
- 前端路由：对于单页面应用程序来说，主要通过URL中的hash(#号)改变来实现不同页面之间的切换，同时，hash有一个特点：HTTP请求中不会包含hash相关的内容；所以，单页面程序中的页面跳转主要用hash实现；

</br>

#### vue-router的基本使用
- 安装可参考官方[中文文档](https://router.vuejs.org/zh/installation.html)，在导入vue-router包之前需要导入vue包，这里使用的是vue-router v3.1.3，vue版本为2.6.11
- 创建组件对象
- 引入router包之后，在路径后面默认会加上'#/'，且在window全局对象中就有了一个 路由的构造函数，在创建时可传递配置对象（例如routes:设置hash以及对应显示的组件），注意属性routes是不带字母'r'
- 在Vue实例中设置router属性为该路由对象的引用
- 在页面中创建容器`<router-view></router-view>`，作为占位符，用于匹配需要显示的组件（注意页面的跳转链接需要加上'#'，表示非真实路径）

```html
<body>
  <div id="app">
    <a href="#/login">登录</a>
    <a href="#/register">注册</a>
    <!--会在这里显示对应的组件-->
    <router-view></router-view>
  </div>

  <template id="login">
    <div>
      登录组件
    </div>
  </template>
  <template id="register">
    <div>
      注册组件
    </div>
  </template>
  <script>
    var login = {template: '#login'}
    var register = {template: '#register'}
    var routerObj = new VueRouter({
      //routers用于设置路由匹配规则，每个路由规则都是一个对象，且有两个必须属性
      //1.path: 表示监听的路由链接地址
      //2.component: 表示path 对应展示的组件，且必须是模板对象，不能是组件的引用名称
      routes: [{
          path: '/login',
          component: login //不是字符串的引用名称，而是对象
        },
        {
          path: '/register',
          component: register
        }
      ]
    })
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      router: routerObj //配置路由
    });
  </script>
</body>
```

</br>

#### `router-link`标签的使用
- 除了在链接跳转添加'#'表示虚拟路径之外，还可以也能使用`<router-link to="/login"></router-link>`直接设置to属性值为路径
- 该标签默认渲染为a标签，可以通过属性tag设置渲染的标签 ，渲染为其他标签一样具备点击切换的效果

```html
<router-link to="/login" tag="span"></router-link>
```

</br>

#### 路由redirect重定向
- 访问根路径时重定向展示登录组件（设置默认展示的组件）

```js
 var routerObj = new VueRouter({
      routes: [
        {path:'/',redirect:'./login'},
        {path: '/login',component: login}
      ]
    })
```

</br>

#### 设置选中路由高亮
- 使用`router-link`切换时，会默认自动给选中的标签添加类'router-link-active'，所以可以在css中定义选中的类样式
- 默认值'router-link-active'可以通过路由的构造选项 linkActiveClass 来全局配置

```js
 var routerObj = new VueRouter({
      routes: [],
      linkActiveClass:'myactive'  //自己定义高亮的类名
    })
```

</br>

#### 设置路由动画
- 将`router-view`放入transition中，并设置transition的mode属性值为"out-in"
- 在样式中设置'.v-enter,.v-leave-to'与'.v-enter-active,.v-leave-active'

```
.v-enter,.v-leave-to {
  opacity: 0;
  transform: translateX(100px);
}

.v-enter-active,.v-leave-active {
  transition: all 0.8s ease;
}
```

</br>

#### 路由规则中定义参数
- 传参方式一
  + 在路由路径中设置参数，可以不用修改路由规则（routes属性）
  + 在组件`this.$route.query`对象中可以获取到传递的参数

```html
<router-link to="/login?id=10&name=hym">登录</router-link>
<script>
    var login = {
      //可以不用带this
      template: '<h3>登陆===>{{$route.query.id}}===>{{$route.query.name}}</h3>',
      created: function () { //组件生命周期钩子函数
        console.log(this.$route.query);
      }
    }
</script>
```

- 传参方式二
  + 修改路由匹配规则，添加路径参数，即在路径后面拼接'/:paramsName'
  + 在`router-link`的to属性值中拼接传递的参数
  + 在组件`this.params`对象中可以获取传递的参数

```html
<router-link to="/login/10/hym">登录</router-link>
<script>
 var login = {
  //可以不用带this
  template: '<h3>登陆===>{{$route.params.id}}===>{{$route.params.name}}</h3>',
  created: function () { //组件生命周期钩子函数
    console.log(this.$route.params);
  }
}
 var routerObj = new VueRouter({
      routes: [{
          path: '/',
          redirect: '/register'
        },
        {
          path: '/login/:id/:name',
          component: login 
        }],
    })
</script>
```

</br>

#### 路由嵌套
- 在父级组件中添加子组件`router-link`以及展示子组件的`router-view`
- 在路由配置对象中找到父组件的配置对象，在'path'、'component'属性之后添加'children:[]'属性，在里面配置子组件的路由
- 子组件路由推荐使用相对路径，即不带'/'，默认拼接父级路径（router-link访问时需要带上父组件路径）

```html
<div id="app">
    <router-link to="/parent">parent</router-link>
    <router-view></router-view>
</div>

<template id="parent">
    <div>
      <h4>这里是父级组件，包含有登录注册子组件</h4>
      <router-link to="/parent/login">登录</router-link>
      <router-link to="/parent/register">注册</router-link>
      <router-view></router-view>
    </div>
</template>
```

```js
var routerObj = new VueRouter({
  routes: [{
    path: '/parent',
    component: parent,
    children: [{
        path: 'login', //会默认拼接父级的路径，所以访问路径为/parent/login，如果使用'/login'则访问路径仍是/login
        component: login
      },
      {
        path: 'register',
        component: register
      }
    ]
  }]
})
var vm = new Vue({
  el: '#app',
  router: routerObj //配置路由
});
```

</br>

#### 命名视图实现经典布局
- 定义页面组件，header、left、main
- 在VueRouter实例配置routes属性：根路径配置components对象，包含多个组件，其中'default'属性放默认组件
- 给页面`router-view`指定name属性为组件对应名称

```html
<div id="app">
    <router-view></router-view>
    <router-view name="left"></router-view>
    <router-view name="main"></router-view>
</div>
<script>
    var header = {template: '<header>这是头部</header>'}
    var left = {template: '<aside>左侧栏</aside>'}
    var main = {template: '<section>主体内容</section>'}
    var routerObj = new VueRouter({
      routes: [{
        path: '/',
        components: {
          'default': header, //不设置name属性则默认渲染header
          'left': left,
          'main': main
        }
      }]
    })
    var vm = new Vue({
      el: '#app',
      router: routerObj //配置路由
    });
</script>
```

</br>

#### 监听文本框改变
- 方式一：keyup实时监听文本输入状态，当输入状态改变则输出相应的改变

```html
<div id="app">
    <input type="text" v-model="first" @keyup="getChange">+
    <input type="text" v-model="second" @keyup="getChange">=
    <input type="text" v-model="full">
</div>
<script>
    var vm = new Vue({
      el: '#app',
      data: {
        first: '',
        second: '',
        full: ''
      },
      methods: {
        getChange() {
          this.full = this.first + '-' + this.second
        }
      }
    });
</script>
```

- 方式二：watch监听data属性值的改变

```js
var vm = new Vue({
  el: '#app',
  data: {
    first: '',
    second: '',
    full: ''
  },
  watch: {
  //监听的data属性（当对应data改变就会触发该函数）
    'first': function (newVal, oldVal) {
      this.full = newVal + '-' + this.second;
    },
    'second': function (newVal) {
      this.full = this.first + '-' + newVal;
    }
  }
});
```

> watch还可以监视路由地址（$route.path）的改变

```js
var vm = new Vue({
  el: '#app',
  watch: {
    '$route.path': function (newUrl) {
      if (newUrl === '/login') {
        console.log('切换到登录组件')
      }
    }
  }
});
```

- 方式三：computed计算属性的使用
  + 可以在computed中定义一些属性即计算属性，计算属性的本质是方法，只不过在使用过程是将其名称当属性来用，不会当成方法来调用
  + 计算属性在被调用的时候不要添加()，而是当成普通属性使用
  + 只要在计算属性的function内引用的任何 data 发生改变，就会重新计算该属性的值
  + 计算属性的求值结果，会被缓存起来方便下一次直接调用，即只要依赖的data没有改变，再次使用计算属性的时候不会执行function，提高性能

```js
var vm = new Vue({
  el: '#app',
  data: {
    first: '',
    second: ''
  },
  computed: {
    //不需要在data中定义full属性
    'full': function () {
      return this.first + '-' + this.second;  //只要data属性值改变就会触发该函数
    }
  },
})
```

</br>

#### watch`、`computed`和`methods`之间的对比

1. `computed`属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；
2. `methods`方法表示一个具体的操作，主要书写业务逻辑；
3. `watch`一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看作是`computed`和`methods`的结合体；