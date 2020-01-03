### Vue指令

#### MVC与MVVM
- MVC应用程序设计模式，Model（模型）一般只负责操作数据库，执行对应的sql语句；View（视图）负责显示数据；Controller（控制器）处理业务逻辑，比如路由分发、从视图层读取数据、向模型发送数据
- MVVM前端视图层的分层开发思想，Model保存的是页面数据，以数据为中心；View是用户看到的页面结构、布局和外观；ViewModel（视图模型）负责视图和数据绑定器之间的通信，即数据的双向绑定

</br>

#### vue安装与使用
- 可以在Vue.js官网查看[安装教程](https://cn.vuejs.org/v2/guide/installation.html)，或者直接在[github](https://github.com/vuejs/vue/releases)上选择对应版本下载
- 在浏览器安装[Vue Devtool扩展](https://github.com/vuejs/vue-devtools#vue-devtools)，方便调试Vue应用

</br>

#### Vue的简单使用
- 引入vue.js `<script src="./lib/vue-2.6.11.js"></script>`
- 准备页面结构
- 创建vue实例，绑定数据

```html
<body>
  <div id="hello">
    {{content}}
  </div>
  <script>
    //创建的vm对象就是视图模型ViewModel
    var vm = new Vue({
      el: '#hello', //Vue实例控制的页面区域
      //data就是Model
      data: { //data属性中存放el需要使用到的数据
        content: 'Hello Vue!'
      }
    })
  </script>
</body>
```

</br>

#### 常用的vue指令
> `v-cloak` 解决插值表达式闪烁的问题
- 案例：避免网速不好时显示{{data}}，当数据渲染完毕会移除元素的v-cloak属性

```html
<style>
   [v-cloak]{display:none;}
</style>

<p v-cloak>{{data}}</p>
```

</br>

> `v-text` 设置元素显示的数据
- 案例： ` <ul v-text='content'></ul>`
- 与插值表达式的不同点
  + v-text不会有闪烁问题
  + 会覆盖元素本身原有的内容

</br>

> `v-html` 设置内容以html代码形式插入
- 案例: `<div v-html='content'></div>` 其中数据 content是代码字符串`<h1>hello vue</h1>`

</br>

> `v-bind:` 绑定元素特性，可以简写为冒号:例如:title=""，元素属性值不能直接设置为data中的变量值
>
> - 案例：绑定title属性，鼠标悬停查看提示 `<span v-bind:title="message+'123'"></span>`    

</br>

> `v-on:`绑定事件比如click、mouseover，可以简写为@，例如@click="show"，可以在方法名后面加括号，达到传参的效果
- 案例：绑定点击事件<div id="hello"><input type="button" :title="点击事件" v-on:click="show" value="点击"></div>

```html
<script>
var vm = new Vue({
  el: '#hello', 
  data: { 
    content: '<h1>hello vue</h1>',
  },
  methods: {
    show: function () {
      alert('点击事件');
    }
  },
})
</script>
```

</br>

#### 简单实现跑马灯效果
- 注意：在vue实例中，如果要获取data中的数据，或者调用methods中的方法，都要通过this.数据属性名或者this.方法名

```js
var vm = new Vue({
  el: '#word',
  data: {
    mes: '滚动的文字，跑马灯效果',
    timer: null
  },
  methods: {
    wordChange: function () {
      let start = this.mes.substring(0, 1); //截取第一个字符
      let remain = this.mes.substring(1); //截取第一个字符以后的字符串
      this.mes = remain + start;
    },
    run: function () {
      if (this.timer != null) return; //防止多次开启定时器
      this.timer = setInterval(() => {
        this.wordChange();
      }, 300)
    },
    stop: function () {
      clearInterval(this.timer);
      this.timer = null; //清除定时器之后记得初始化变量
    }
  },
})
```

</br>

> vue指令`v-on`的缩写以及事件修饰符（可以连缀）

+ .stop       阻止冒泡（阻止从内到外的事件冒泡，设置在内层元素）

+ .prevent    阻止默认事件（比如a链接触发事件但不跳转）

+ .capture    添加事件侦听器时使用事件捕获模式（从外到内的捕获阶段触发事件，设置到外层）

+ .self       只当事件在该元素本身（比如不是子元素）触发时触发回调，即只是自身元素冒泡捕获事件不触发，但不会阻止外层元素的冒泡事件，也就是不会阻止冒泡行为

+ .once       事件只触发一次，比如`<a href="www.baidu.com" @click.prevent.once="aClick">百度</a>`只会触发一次阻止默认行为和aClick事件

案例：
```html
<div class="bg" @click="bgClick">
  <input type="button" value="按钮" @click.stop="btnClick">
</div>

<script>
    var vm = new Vue({
      el: '#word',
      data: {
        mes: 'word~'
      },
      methods: {
        bgClick() {
          console.log("触发背景的事件");
        },
        btnClick() {
          console.log("触发按钮的事件");
        }
      },
    })
</script>
```

</br>


> Vue指令之`v-model`和双向数据绑定

- `v-bind:`只能实现数据的单向绑定，从 M 自动绑定到 V，不能实现数据双向绑定（比如修改input的value属性值，不会改变model层的数据）

- `<h4>{{formText}}</h4> <input type="text" v-model="formText">`此时h4标签和input中的内容会同时改变

- 注意：v-model只能运用在表单元素中，比如input、select、text、radio、checkbox、textarea、address···

</br>

> `v-for`循环（在组件使用v-for需要用v-bind:key 绑定元素，保证唯一性，且key只能是string或者number）
- 遍历数组

```html
<ul id="word"><li v-for="(item,index) in list" :key="item.name">{{item.name}}</li></ul>
<script>
    var vm = new Vue({
      el: '#word',
      data: {
        list: [{
            name: 'hym',
            age: 21,
            sex: 'men'
          },
          {
            name: 'pjj',
            age: 22,
            sex: 'women'
          }
        ]
      }
    })
</script>
```

- 遍历对象

```html
<ul id="word">
  <li v-for="(val,key,index) in objInfo">{{key}}：{{val}} 索引值为{{index}}</li>
</ul>

<script>
    var vm = new Vue({
      el: '#word',
      data: {
        objInfo: {
          name: 'hym',
          age: 21,
          sex: 'men'
        }
      }
    })
</script>
```

</br>

> `v-if`与`v-show`

- 两者都是flag为true的时候显示，v-if控制的元素会进行DOM的创建或删除，v-show控制的元素只是添加或移除`display:none`的属性 
- `v-if`有较高的切换性能消耗（元素创建但可能永远不显示推荐使用）
- `v-show`有较高的初始渲染消耗（涉及元素频繁隐藏显示时推荐使用）

```html
<h4 v-if="flag">v-if的使用</h4>
<h4 v-show="flag">v-show的使用</h4>
```

- 迭代数字（从 1 开始）

```html
<p v-for="i in 10">这是第 {{i}} 个P标签</p>
```

</br>

#### 在vue中设置样式
> 设置class

- 数组，class用v-bind:进行数据绑定，相当于 `class="red thin"`

```html
<h1 :class="['red', 'thin']">其中red和thin是设置完的样式类</h1>
```

- 数组中使用三元表达式

```html
<h1 :class="['red', 'thin', isactive?'active':'']">其中isactive是data中的布尔变量</h1>
```

- 数组中嵌套对象

```html
<h1 :class="['red', 'thin', {'active': isactive}]">使用对象的形式提高代码的可读性</h1>
```

- 直接使用对象

```html
<h1 v-bind:class="{red:true, italic:true, active:true, thin:true}">
不包含特殊符号的对象属性可以带引号也可以不带引号；
还可以将该对象定义为data中的变量，直接引用变量
</h1>
```

</br>

> 设置内联样式

- 直接在元素上通过 `:style` 的形式，书写样式对象（键值对，注意带符号-的属性要加引号）

```html
<h1 :style="{color: 'red', 'font-size': '40px'}">这是一个善良的H1</h1>
```

- 将样式对象，定义到 `data` 中，并将变量直接引用到 `:style` 中

- 在 `:style` 中通过数组，引用多个 `data` 上的样式对象

```html
<h1 :style="[h1StyleObj, h1StyleObj2]">其中h1StyleObj与h1StyleObj2是data中定义的变量</h1>
```

</br>

#### 过滤器
> Vue.js 允许你自定义过滤器，**可被用作一些常见的文本格式化**，源数据不改变。过滤器可以用在两个地方：**mustache 插值和 v-bind 表达式**。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符指示；

##### 全局过滤器
> 使用ES6中的字符串新方法 String.prototype.padStart(maxLength, fillString='') 或 String.prototype.padEnd(maxLength, fillString='')来填充字符串；

```html
<!-- 可以调用多个过滤器 -->
 <td>{{item.time | dateFormat('yyyy-mm-dd') | nextcahnge}}</td>

<script>
  Vue.filter('dateFormat', (date, pattern = '') => {
      let dt = new Date(date);
      let y = dt.getFullYear();
      let m = dt.getMonth() + 1;
      let d = dt.getDate();
      //避免未传参报错，需要设置pattern参数的默认值
      if (pattern.toLowerCase() == 'yyyy-mm-dd') {
        //返回年月日
        // return y + '-' + m + '-' + d;
        return `${y}-${m}-${d}`
      } else {
        //返回时分秒
        let hh = dt.getHours().toString().padStart(2, '0'); //第一个参数是补全后的字符长度
        let mm = dt.getMinutes().toString().padStart(2, '0');
        let ss = dt.getSeconds().toString().padStart(2, '0');
        return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
      }
    })
    
    Vue.filter('nextChange', (data) => { 
      return data+"调用第二个过滤器";
    })
</script>
```

##### 私有过滤器
- html元素

```html
<td>{{item.ctime | dataFormat('yyyy-mm-dd')}}</td>
```

- 私有 `filters` 定义方式

```js
var vm = new Vue({
    el:'#app',
    data:{},
    filters:{  /// 私有局部过滤器，只能在 当前 VM 对象所控制的 View 区域进行使用
        dateFormat(dateStr, pattern = '') {//通过 pattern="" 来指定形参默认值，防止报错}  
    }
})

```

</br>

#### 自定义按键修饰符
- 关于按键修饰符可以查看[官网介绍](https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6)
- 案例：按下回车键触发事件`<input type="text" v-model="name" @keyup.enter="add">`，直接用码值13代替enter效果相同，同理对应其他按键比如f2（113）都可以用对应码值绑定
- 自定义全局按键修饰符`Vue.config.keyCodes.f2 = 113;`

```html
<input type="text" v-model="name" @keyup.f2="add">
```

</br>

#### 自定义指令（指令名称使用小写字母）
- 详细可以查阅[官网自定义指令介绍](https://cn.vuejs.org/v2/guide/custom-directive.html)
- `Vue.directive()`定义全局的指令，第一个参数是不包含'v-'的指令名称（但调用的时候需要加v-），第二个参数是一个对象，包含一些指令的相关函数
- 自定义文本框自动获取焦点指令`v-focus`

```html
<input type="text" v-model="keywords" v-focus>

<script>
 //自动聚焦到文本框
    Vue.directive('focus', {
      //所有方法第一个参数永远是el，表示被绑定指令的元素，是原生的JS对象
      bind: function (el) { //当指令绑定到元素上的时候，立即执行bind函数，只执行一次

        //el.focus();绑定指令的时候，元素还没有插入到DOM 中，无法获取焦点，所以调用focus方法没有用
      },
      inserted: function (el) { //元素插入到Dom的时候，会执行inserted函数，触发一次
        el.focus(); //原生JS对象
      },
      updated: function (el) { //当VNode更新的时候，会执行updated，可能会触发多次

      }
    })
</script>
```

- 自定义设置样式的指令
> 和样式相关的操作，一般都可以在bind执行

```html
<a href="javascript:;" v-color v-fontweight="600">删除</a>
<script>
//自定义设置字体颜色的指令
Vue.directive('color', {
  //只要通过指令绑定了元素，无论元素有没有插入到页面中，这个元素就有了一个内联的样式，显示的时候也就必然会解析样式

  bind: function (el, binding) {
    el.style.color = 'red';
    //指令名称、指令绑定的值v-color="值"、指令绑定的表达式"1+1"
    // console.log(binding.name + '' + binding.value + '' + binding.expression)
  }
})

var vm = new Vue({
    el:'#app',
    data:{},
    methods:{},
    filters:{},
    directives: {
        'fontweight': {
          bind: function (el, binding) {
            el.style.fontWeight = binding.value;
          }
        },
        'fontsize': function (el, binding) { //注意 这里的function等同于将代码写到bind和update中
          //v-fontsize="'18px'"调用的时候可带可不带单位，但带单位要用字符串
          el.style.fontSize = parseInt(binding.value) + 'px';
        }
     }
})
</script>
```


</br>

#### 基于vue的应用案例
> 使用到的第三方文件
- vue.js：可以在官网或者[github](https://github.com/vuejs/vue/releases)下载对应版本，这里使用的是2.6.11
- BootStrap框架的css：同样可以在[官网](https://www.bootcss.com/)或者[github](https://github.com/twbs/bootstrap)下载对应版本，这里使用的是3.3.7
- 在项目目录vueBranList下创建文件夹lib，将vue-2.6.11.js和bootstrap.min.css放入文件夹中

> 功能实现
- 准备虚拟的列表数据（对象数组）brandList
- 借助BootStrap快速搭建静态页面（v-for使用时设置key）
- 实现添加（v-model的使用）和删除功能（传参、阻止默认事件）
- 搜索关键字功能（渲染页面的数据根据keyword改变，应该从带参方法中动态获取数据）
- 代码实现：

```html
<body>
  <div id="app">
    <div class="panel panel-primary">
      <div class="panel-heading">
        <h3 class="panel-title">添加品牌</h3>
      </div>
      <div class="panel-body form-inline">
        <label for="id">
          ID：
          <input type="text" name="id" class="form-control" v-model="id">
        </label>
        <label for="name">
          Name:
          <input type="text" name="name" class="form-control" v-model="name">
        </label>
        <input type="button" value="添加" class="btn btn-primary" @click="add">
        <label for="keywords">
          搜索关键字：
          <input type="text" name="keywords" class="form-control" v-model="keywords" v-focus>
        </label>
      </div>
    </div>
    <table class="table table-bordered table-hover table-striped">
      <thead>
        <tr>
          <th>id</th>
          <th>name</th>
          <th>create time</th>
          <th>opration</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in search(keywords)" :key="item.id">
          <td>{{item.id}}</td>
          <td>{{item.name}}</td>
          <td>{{item.time | dateFormat}}</td>
          <td>
            <a href="javascript:;" @click.prevent="del(item.id)">删除</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>

  <script>
    //自动聚焦到文本框
    Vue.directive('focus', {
      //所有方法第一个参数永远是el，表示被绑定指令的元素，是原生的JS对象
      inserted: function (el) { //元素插入到Dom的时候，会执行inserted函数，触发一次
        el.focus(); //原生JS对象
      }
    })
    var vm = new Vue({
      el: "#app",
      data: {
        id: '',
        name: '',
        keywords: '',
        brandList: [{ //页面渲染的静态数据
            id: 1,
            name: 'MAC',
            time: new Date()
          },
          {
            id: 2,
            name: 'Chanel',
            time: new Date()
          },
          {
            id: 3,
            name: 'TF',
            time: new Date()
          }
        ]
      },
      methods: {
        add() {
          let item = {
            id: this.id,
            name: this.name,
            time: new Date()
          };
          this.brandList.push(item);
          this.id = '';
          this.name = '';
        },
        del(id) {
          //1.通过some方法，在return true的时候停止后续循环
          /*  this.brandList.some((item, index) => {
             if (item.id === id) {
               this.brandList.splice(index, 1);
               return true;
             }
           }) */

          //2.通过新方法findIndex，在return true的时候返回对应的索引值
          let index = this.brandList.findIndex(item => {
            if (item.id === id) {
              return true;
            }
          })
          this.brandList.splice(index, 1);
        },
        search(keywords) {
          //1.遍历查找包含keywords的name，将对应数据放入新数组中
          /* let newList = [];
          this.brandList.forEach((item, index) => {
            if (item.name.indexOf(keywords) != -1) {
              newList.push(item);
            }
          })
          return newList; */

          //2.filter过滤符合条件的item，方法返回数组
          return this.brandList.filter(item => {
            if (item.name.includes(keywords)) {
              return item;
            }
          })
        }
      },
      filters: {
        dateFormat(dateStr, pattern = '') {
          let dt = new Date(dateStr);
          let y = dt.getFullYear();
          let m = dt.getMonth() + 1;
          let d = dt.getDate();

          if (pattern.toLowerCase() == 'yyyy-mm-dd') {
            return `${y}-${m}-${d}`
          } else {
            let hh = dt.getHours().toString().padStart(2, '0'); //第一个参数是补全后的字符长度
            let mm = dt.getMinutes().toString().padStart(2, '0');
            let ss = dt.getSeconds().toString().padStart(2, '0');
            return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
          }
        }
      },
      directives: {
        'fontweight': {
          bind: function (el, binding) {
            el.style.fontWeight = binding.value;
          }
        },
        'fontsize': function (el, binding) { //注意 这里的function等同于将代码写到bind和update中
          //v-fontsize="'18px'"调用的时候可带可不带单位，但带单位要用字符串
          el.style.fontSize = parseInt(binding.value) + 'px';
        }
      }
    })
  </script>
</body>
```

## 相关文章

1. [vue.js 1.x 文档](https://v1-cn.vuejs.org/)
2. [vue.js 2.x 文档](https://cn.vuejs.org/)
3. [String.prototype.padStart(maxLength, fillString)](http://www.css88.com/archives/7715)
4. [js 里面的键盘事件对应的键码](http://www.cnblogs.com/wuhua1/p/6686237.html)
5. [Vue.js双向绑定的实现原理](http://www.cnblogs.com/kidney/p/6052935.html)