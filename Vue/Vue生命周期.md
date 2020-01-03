### Vue生命周期

#### Vue实例的生命周期
> 从Vue实例创建、运行、到销毁期间，总是伴随着各种各样的事件，这些事件，统称为生命周期。

- [生命周期钩子](https://cn.vuejs.org/v2/api/#选项-生命周期钩子)：就是生命周期事件的别名；生命周期钩子 = 生命周期函数 = 生命周期事件
- 创建期间的生命周期函数:
  + `beforeCreate`：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性
  + `created`：实例已经在内存中创建OK，此时 data 和 methods 已经创建OK，此时还没有开始 编译模板
  + `beforeMount`：此时已经完成了模板的编译，但是还没有挂载到页面中
  + `mounted`：此时，已经将编译好的模板，挂载到了页面指定的容器中显示

- 运行期间的生命周期函数：
  + `beforeUpdate`：状态更新之前执行此函数，此时data中的状态值是最新的，但是界面上显示的数据还是旧的，因为此时还没有开始重新渲染DOM节点
  + `updated`：实例更新完毕之后调用此函数，此时data中的状态值和界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了！

- 销毁期间的生命周期函数：
  + `beforeDestroy`：实例销毁之前调用。在这一步，实例仍然完全可用。
  + `destroyed`：Vue 实例销毁后调用。调用后，Vue实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁

</br>

#### 创建期间的生命周期函数
- 在beforeCreate生命周期函数执行的时候，data和methods中的数据都还没有初始化，不能被调用
- 在created生命周期函数中已经可以使用data和methods
- 在beforeMount生命周期函数中，还不能通过DOM 操作获取到元素真实内容
- 在mounted是实例创建的最后一个生命周期函数

```js
var vm = new Vue({
  el: '#app',
  data: {
    msg: '数据初始化'
  },
  methods: {
    show() {
      console.log('调用show方法');
    }
  },
  beforeCreate() { //第一个生命周期函数
    //此时已经初始化了空的Vue实例对象，只有默认事件与生命周期函数，不能使用data和methods
  },
  created() { //第二个生命周期函数
    //此时data和methods已经初始化，可以调用
    console.log(this.msg);
    this.show();
  },
  beforeMount() { //第三个生命周期函数，模板只是在内存中编译完成
    //此时Vue已经执行了指令，在内存中生成编译好的最终模板字符串，且将其渲染为内存中的DOM，
    //但只是在内存中渲染好了模板，并没有把编译好的模板挂载到真正的页面中

    console.log(document.getElementById('test').innerText) //只能打印{{msg}}，页面元素还没有被替换
  },
  mounted() {//第四个生命周期函数，这是实例创建的最后一个生命周期函数
    //此时编译好的模板已经挂载到页面中，可以通过DOM操作获取页面元素真实内容
  }
})
```

</br>

#### 组件运行期间的生命周期函数
> beforeUpdate和updated根据数据的改变执行0到无数次

- beforeUpdate 表示界面还没有更新，但是数据data已经更新了
- updated 此时页面数据和data中的数据已经保持一致

```js
var vm = new Vue({
      el: '#app',
      data: {
        msg: '数据初始化'
      },
      methods: {},
      beforeUpdate() { //触发事件改变msg的时候，在该生命周期函数内界面内容为更改前，data中内容为更改后
        console.log('界面中的元素内容：' + document.getElementById('test').innerText);
        console.log('data中的内容：' + this.msg);
      },
      updated() { //根据data最新数据在内存中渲染出一份最新的DOM树，然后将它渲染到了真实页面中，此时已经完成了Model到View的更新
        console.log('界面中的元素内容：' + document.getElementById('test').innerText);
        console.log('data中的内容：' + this.msg);
      },
    })
```

</br>

#### 销毁期间的生命周期函数
- `beforeDestroy`执行的时候，Vue实例已经从运行阶段，进入到销毁阶段，此时实例上所有的data和methods以及filters、指令等都处于可用状态，还没有真正执行销毁
- `destroyed`执行的时候，实例上所有的data和methods以及filters、指令等都不可用