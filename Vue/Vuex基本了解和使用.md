### Vuex基本了解和使用

#### 为什么使用Vuex

- Vuex是专为Vue.js应用程序开发的状态管理模式，方便任何组件直接获取或修改公共数据,具体可以参考[官网](https://vuex.vuejs.org/zh/)
- 保存组件间共享数据,可以将共享数据挂载到vuex中，而不需要通过父子组件间传值（只有共享数据才挂载到vue中）
- vuex是一个全局的共享数据存储区域，就相当于是一个数据的仓库；而props用于存放从父组件传递的值；data则存放内部私有的数据

</br>

#### 安装使用vuex
- `npm install vuex --save`
- 导入和注册包`import Vuex from 'vuex'` `Vue.use(Vuex)`
- 组件页面中通过$state.变量名访问，或者在store中的getters包装数据之后对外提供
- 创建数据存储对象，在组件中可以this.$store.state.变量名访问，但是不符合设计理念；正确使用是通过调用mutation提供的方法来操作对应的数据，不推荐直接操作，容易导致数据紊乱，且不能定位到错误的原因
  + 在mutation方法中，通过方法形参state传递，然中通过state.变量名访问state中的变量；额外的参数只能有一个，即第二个，如果希望传递多个的时候可以传递对象
  + 组件中使用mutation的方法只能使用this.$store.commit('方法名')来触发

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex);

var store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment(state,c){
        state.count+=c;  //c是方法调用时传递的额外参数
    }
  }，
  getters: {
    newCount: function (state) {
      return '获取当前值' + state.count
    }
  }
})

import App from './App.vue'
const vm = new Vue({
  el: '#app',
  render: c => c(App),
  store
})
```

</br>

#### 总结
- 如果想要修改state中的数据，要通过mutations提供的方法，组件中通过this.$store.commit('方法名称','唯一的参数')
- 组件中直接访问数据可以通过this.$store.state.变量名获取
- 当store中数据对外提供的时候需要进行一层封装就通过getters，对外提供方法，组件中则通过this.$store.getters.方法名访问数据