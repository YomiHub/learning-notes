> 组件是为了拆分vue实例，让我们可以用不同组件来划分不同功能模块，当用到功能时只需要调用对应的组件即可
- 模块化：从代码逻辑上划分，方便代码的分层开发，保证每个功能模块功能单一
- 组件化：从UI角度进行划分，前端的组件化，方便UI组件的重用

</br>

#### 全局组件定义
> 注意： 组件中的DOM结构，有且只能有唯一的根元素（Root Element）来进行包裹！例如：当需要定义模板为多个并列元素，则需要在外层用一个元素包裹

- （1）使用Vue.extend配合Vue.component
  + 定义页面展示的HTML结构
  + 设置组件名称（注意：使用驼峰命名时，页面调用组件要用小写字母并用横线）

```js
var tem = Vue.extend({
  template: '<h3>这是组件要展示的html结构，通过template属性指定</h3>'
})

Vue.component('myComp', tem); //驼峰，所以页面调用时需要将大写字母改为小写并加横线- 即<my-comp>
//Vue.component('mycomp',tem);  //小写字母，直接使用标签<mycomp>
```

- （2）直接使用 Vue.component 方法：

```js
Vue.component('myComp', {
  template: '<h1>直接用Vue.component</h1>'
});
```

- （3）在script标签中定义模板字符串 或者 在 #app 控制外，用template定义，再结合Vue.component定义组件

```html
<!--方式一：在script标签内-->
<script id="mytem" type="x-template">
<h3>这是组件要展示的html结构，通过script标签定义</h3>
</script>

<!--方式二：在template标签内-->
<div id="app">
    <my-comp></my-comp>
</div>
<template id="mytem">
<h3>这是组件要展示的html结构，通过template标签定义</h3>
</template>
```

```js
Vue.component('myComp', {
  template: '#mytem'
});
```

</br>

#### 定义私有组件（只能在指定Vue实例中使用）
- 可以用id指定template也可以直接使用标签字符串

```js
var vm = new Vue({
  el: "#app",
  data: {},
  methods: {},
  filters: {},
  directives: {},
  components: {
    myComp: {
      template: '#mytem'
    }
  }
})
```

- 可以直接在components中引用对象，页面调用的时候直接用变量名称<mytem></mytem>

```js
var mytem = {
    template:'<h3>这是HTML结构</h3>'
}

var vm = new Vue({
  el: "#app",
  components: {
    mytem   //即属性名和属性值为mytem
  }
})
```

- 可以在子组件中定义子组件

```js
components: { // 定义子组件（私有组件）
    account: { // account 组件
      template: '<div><h1>这是Account组件{{name}}</h1><login></login></div>', // 在这里使用定义的子组件
      components: { // 定义子组件的子组件
        login: { // login 组件
          template: "<h3>这是登录组件</h3>"
        }
      }
    }
}
```

</br>

#### 组件中的数据和响应事件
- 组件中的data被定义为一个方法，且该方法必须返回一个对象，让每个组件都有独立的数据（组件中的data和实例中data使用方式一样）

```js
Vue.component('myComp', {
  template: '<h3>这是组件要展示的html结构，通过template标签定义===>{{msg}}</h3>',
  data: function () {
    return {
      msg: '这是组件中的data'
    }
  }
});
```

- 组件中方法的定义和使用与实例一样

```js
Vue.component('myComp', {
  template: '#mytem',
  data: function () {
    return {
      count: 0
    }
  },
  methods: {
    increment() {
      this.count++; //调用组件data时要用this
    }
  },
});
```

</br>

#### 组件切换
- 方式一：使用`v-if`与`v-else`结合data标识符切换（缺陷：不能用于三个或三个组件之间的切换）

```html
<div id="app">
    <a href="" @click.prevent="flag=true">组件一</a>
    <a href="" @click.prevent="flag=false">组件二</a>
    <my-com1 v-if="flag"></my-com1>
    <my-com2 v-else="flag"></my-com2>
  </div>
```

- 方式二：用component标签占位符结合`:is`属性设置组件名称来切换组件（注意：直接设置组件名称要带引号标识为字符串，且可以用驼峰）

```html
<div id="app">
    <!--字符串或者变量设置对应名称的组件-->
    <!--<component :is="'myComp'"></component>-->
    <a href="" @click.prevent="compName='myComp1'">comp1</a>
    <a href="" @click.prevent="compName='myComp2'">comp2</a>
    <component :is="compName"></component>
</div>
```

</br>

#### 组件切换动画
> 比起普通动画需要多设置一个动画切换方式的属性`mode`，可参考官网[文档链接](https://cn.vuejs.org/v2/guide/transitions.html#%E5%A4%9A%E4%B8%AA%E7%BB%84%E4%BB%B6%E7%9A%84%E8%BF%87%E6%B8%A1)

```html
<style>
.fade-enter,
.fade-leave-to {
  opacity: 0;
  transform: translateY(150px);
}

.fade-enter-active,
.fade-leave-active {
  transition: all 1s ease;
}
</style>
<!--通过设置mode="out-in"让组件先隐藏之后，另一个组件再显示-->
<transition name="fade" mode="out-in">
  <component :is="compName"></component>
</transition>
```

</br>

##### 父组件向子组件传值
> 注意：在标签中自定义属性不能用驼峰，需要将大写转为小写并加横线'-'；另外在props数组定义的属性要加引号
- 父组件可以在引用 子组件 的时候，通过属性绑定（v-bind:）的形式，把需要传递给 子组件 的数据以属性的形式传递
- 在 子组件 的props数组中定义父元素传，递数据的属性，此时就可以在 子组件 中正常使用变量

```html
 <my-comp v-bind:msg-by-parent="msg"></my-comp>
```

```js
var vm = new Vue({
  el: "#app",
  data: {
    compName: 'myComp1',
    msg: '这是Vue实例中的msg'
  },

  components: {
    myComp: {
      template: '<div>这是模板的内容===>{{msgByParent}}</div>',
      props: ['msgByParent']
    }
  }
})
```

</br>

#### 父组件向子组件传递方法
> 注意使用`v-on:`绑定方法的时候不能使用驼峰和横线'-'，推荐使用小写字母和数字
- 在调用组件时用`v-on:`绑定方法属性
- 在子组件的时间方法中通过`this.$emit()`触发对应方法，注意方法名是字符串形式

```html
<my-comp v-on:parentfunc="show"></my-comp>
```

```js
var vm = new Vue({
  el: "#app",
  data:{
      msgFromSon:null
  },
  methods: {
    show(data) {
      this.msgFromSon = data;
      console.log('这是Vue实例的show方法，参数1为'+data)
    }
  },
  components: {
    myComp: {
      template: '<div @click="run">点击调用父组件show方法</div>',
      methods: {
        run() {
          //emit原译：触发、调用
          this.$emit('parentfunc',this.sonMsg);   //
        }
      },
      data:{
          return {
              sonMsg:{msg:'这是子组件的数据'}
          }
      }
    }
  },
})
```

</br>

#### ref获取DOM元素和组件引用
- 给dom元素或者模板元素设置属性ref，在Vue实例方法中可以通过`this.refs.属性值`获取到dom或者组件元素
- 还可以通过获取到的组件元素调用组件上的data和methods

```html
<div id="refTest">
    <input type="button" value="获取DOM和组件元素" @click="getEle">
    <p ref="domcontent">这是页面元素内容</p>
    <refcomp ref="componentref"></refcomp>
</div>
```

```js
 var refTest = new Vue({
  el: '#refTest',
  data: {},
  methods: {
    getEle() {
      console.log(this.$refs.domcontent);
      console.log(this.$refs.componentref.msg);
      this.$refs.componentref.show(); //调用子组件的方法
    }
  },
  components: {
    refcomp: {
      template: '<h3>这是组件的元素</h3>',
      data: function () {
        return {
          msg: '通过获取dom元素获取组件的data'
        }
      },
      methods: {
        show() {
          console.log('通过获取dom元素触发子组件的方法')
        }
      }
    }
  }
})
```

</br>

#### 使用Vue组件实现发表评论的功能
> 使用到2.6.11版本的vue框架以及3.3.7版本的bootStrap.css；案例中评论内容存储到localStorage
- 组织一个最新的评论数据对象，又因为localStorage 只支持存放字符串数据，所以要先调用 JSON.stringify
- 保存新数据前需要从localStorage 中获取到之前的评论数据，转换为一个数组（如果原来评论为空字符则返回空数组，防止JSON.parse格式化报错）
- 把最新的数据unshift到数组中，再调用JSON.stringfy 转为数组字符串，然后调用localStorage.setItem()

```html
<body>
  <div id="app">
    <commentmodul v-on:reinnit="innitComment"></commentmodul>
    <ul class="list-group">
      <li class="list-group-item" v-for="item in list" :key="item.id">
        <span class="badge">评论者：{{item.user}}</span>
        {{item.comment}}
      </li>
    </ul>
  </div>
  <template id="tplcomment">
    <div>
      <div class="form-group">
        <label for="">
          评论者：
          <input type="text" v-model="user" class="form-control">
        </label>
      </div>
      <div class="form-group">
        <label for="">
          评论内容：
          <textarea cols="30" rows="10" v-model="comment" class="form-control"></textarea>
        </label>
      </div>
      <div class="form-group">
        <input type="button" value="提交评论" class="btn btn-primary" @click="addComment">
      </div>
    </div>
  </template>
  <script>
    var tplcomment = {
      template: '#tplcomment',
      data: function () {
        return {
          user: '',
          comment: ''
        }
      },
      methods: {
        addComment() {
          console.log(Date.now()) //获取1970年1月1日截止到现在时刻的时间戳
          var newComment = {
            id: Date.now(),
            user: this.user,
            comment: this.comment
          };
          var coms = JSON.parse(localStorage.getItem('coms') || '[]');
          coms.unshift(newComment);
          localStorage.setItem('coms', JSON.stringify(coms)); //注意localStorage只能存储字符串
          this.user = '';
          this.comment = '';
          this.$emit('reinnit'); //在提交评论之后需要刷新列表
        }
      },
    }
    var vm = new Vue({
      el: '#app',
      data: {
        list: [{
          id: 1,
          user: 'hym',
          comment: '什么是爱呢？'
        }, {
          id: 2,
          user: 'll',
          comment: '你会喜欢我吗？'
        }]
      },
      methods: {
        //刷新评论列表
        innitComment() {
          var comment = JSON.parse(localStorage.getItem('coms'));
          this.list = comment;
        }
      },
      components: {
        commentmodul: tplcomment
      },
      created: function () {
        this.innitComment; //在created中data和methods才可以用
      }
    })
  </script>
</body>
```