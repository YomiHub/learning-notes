### vue中的动画

> 具体可以参考[官网介绍](https://cn.vuejs.org/v2/guide/transitions.html)

#### 使用过度类名实现动画（共用类名）
- 使用transition元素将需要被动画控制的元素包裹起来
- 自定义两组样式，来控制transition内部的元素实现动画`.v-enter,.v-leave-to`、`.v-enter-active,v-leave-active`

```html
<style>
.v-enter,
.v-leave-to {
  opacity: 0;
  /*  从右侧进入 */
  transform: translateX(150px);

}

.v-enter-active,
.v-leave-active {
  transition: all 0.8s ease;
}
</style>
<input type="button" value="toggle" @click="flag=!flag">
<transition>
  <h3 v-if="flag">展示过渡动画</h3>
</transition>
```

<br>

#### 自定义v-前缀（定义私有动画）
- 给transition元素添加name属性，区分不同的动画，在定义类样式时将'v-替换为'name-'

```html
<style>
.fade-enter,
.fade-leave-to {
  opacity: 0;
  /*  从右侧进入 */
  transform: translateX(150px);

}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.8s ease;
}
</style>
<transition name="fade">
  <h3 v-if="flag">展示过渡动画</h3>
</transition>
```

</br>

#### 使用第三方css动画库animate.css
> 动画库animate.css的[github链接](https://github.com/daneden/animate.css)
- 导入动画类库 `<link rel="stylesheet" type="text/css" href="./lib/animate.css">`
- 定义transition及属性，给动画元素添加'animated'类

```html
<transition
	enter-active-class="fadeInRight"
    leave-active-class="fadeOutRight"
    :duration="{ enter: 500, leave: 800 }">
  	<div class="animated" v-show="isshow">css动画库实现动画</div>
</transition>
```

</br>

#### 使用动画钩子函数
> 使用类名动画和第三方动画库都只能实现全场动画（进入和离开），而使用钩子函数就可以实现半场动画
- 案例：点击按钮，小球运动到右下角后消失（注意:el.offsetWidth与done）

```html
<body>
  <div id="app">
    <input type="button" value="触发小球半场动画" @click="flag = !flag">
    <transition @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
      <div class="ball" v-show="flag"></div>
    </transition>

  </div>
  <script>
    var vm = new Vue({
      el: '#app',
      data: {
        flag: false
      },
      methods: {
        //动画开始前，初始化小球的位置，其中el是原生DOM对象
        beforeEnter(el) {
          el.style.transform = "translate(0,0)";
        },
        enter(el, done) {
          el.offsetWidth; //强制刷新，否则动画效果无法显示，同样可以使用el.offsetLeft|offsetRight|offsetHeight
          //动画终止，定义小球结束动画的位置，以及过渡的状态
          el.style.transform = "translate(150px,450px)";
          el.style.transition = "all 1s ease";
          done(); //是afterEnter函数的引用，调用则表示结束动画后立即执行该函数，不使用会有延迟
        },
        afterEnter(el) {
          //动画结束后的操作，隐藏小球
          this.flag = !this.flag;
        }
      }
    })
  </script>
</body>

```

</br>

#### 实现列表动画
> 当动画的元素是通过`v-for`渲染出来的时候，要用`transition-group`包裹住动画元素
> 给`transition-group`元素添加'appear'属性，让列表显示时有逐渐靠拢的动效
> 使用`transition-group`时需要添加'tag'属性，标明嵌入的标签，避免结构混乱。例如渲染ul列表则设置`tag="ul"`，不指定则会默认将`transition-group`元素渲染为span 元素

```css
.v-enter,
.v-leave-to {
  opacity: 0;
  transform: translateY(70px);
}

.v-enter-active,
.v-leave-active {
  transition: all 1s ease;
}

/* 移除元素时的动画，被删除元素后续的其它元素也产生动画，看起来动画不会生硬
必须和.v-leave-active{position:absolute}一起使用,需要给元素指定固定宽 */
.v-move {
  transition: all 1s ease;
}

.v-leave-active {
  position: absolute;
}
```

```html
<body>
  <div id="app">
    <label for="">
      id:
      <input type="text" name="id" id="" v-model="id">
    </label>
    <label for="">
      name:
      <input type="text" name="name" id="" v-model="name">
    </label>
    <input type="button" value="添加" @click="add">

    <transition-group appear tag="ul">
      <li v-for="(item,index) in list" :key="item.id" @click="del(index)">
        {{item.id}}====={{item.name}}
      </li>
    </transition-group>

  </div>
  <script>
    var vm = new Vue({
      el: '#app',
      data: {
        id: '',
        name: '',
        list: [{
          id: 1,
          name: 'hym'
        }]
      },
      methods: {
        add() {
          this.list.push({
            id: this.id,
            name: this.name
          })
        },
        del(index) {
          this.list.splice(index, 1);
        }
      }
    })
  </script>
</body>
```