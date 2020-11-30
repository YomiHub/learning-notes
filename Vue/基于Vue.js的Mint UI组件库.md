### 基于Vue.js的Mint UI组件库

> Mint UI是基于vue.js的移动端组件库，可参考官方文档[链接]("https://mint-ui.github.io/#!/zh-cn")或者github仓库[地址]("https://github.com/ElemeFE/mint-ui")

#### Mint UI 安装与引入
 - 安装：`npm install mint-ui -S`
 - 完整引入，在main.js中写入如下：

    ```js
    import Vue from 'vue'
    //导入Mint-UI，即导入所有的组件
    import MintUI from 'mint-ui'
    //导入样式表，从包里面导入文件的时候，可以省略node_modules这层目录
    import 'mint-ui/lib/style.css'
    import App from './App.vue'
    //将Mint UI 安装到Vue中，即将所有的组件注册为全局的组件
    Vue.use(MintUI)
    
    new Vue({
      el: '#app',
      components: { App }
    })
    ```
 - 按需引入，借助 babel-plugin-component，我们可以只引入需要的组件
   + `npm install babel-plugin-component -D`
   + 修改 .babelrc（presets部分按自己的需要配置，plugins添加如下数组，参考官网[链接]("https://mint-ui.github.io/docs/#/zh-cn2/quickstart")）
   
   ```
    {
      "presets": ["@babel/preset-env", "@babel/preset-react", "mobx"],
      "plugins": [
        "@babel/plugin-proposal-object-rest-spread",
        "@babel/plugin-transform-runtime",
        ["component", {
          "libraryName": "mint-ui",
          "style": true
        }]
      ]
    }
   ```
   + 如果需要引入button和cell则在main.js中导入
   ```
   import {Button,Cell} from 'mint-ui'
   
   Vue.component(Button.name,Button)  //注册按钮组件，第一个参数是组件名称，可以使用自定义字符串'mybtn'
   Vue.component(Cell.name,Cell)
   /* 或写为
    * Vue.use(Button)
    * Vue.use(Cell)
    */
   ```

#### Mint UI 中按钮组件的使用
- 在组件.vue文件中，通过<mt-button>标签创建按钮
  + 通过type控制按钮样式
  + size控制按钮大小
  + disabled设置为不可点击
  + plain设置为镂空描边的幽灵按钮
  + icon设置按钮图标
- 可以通过@click.native="handleclick"添加点击事件，具体使用参考[链接]("https://mint-ui.github.io/docs/#/zh-cn2/button")

  ```html
    <mt-button type="primary" icon="back" @click="show">
      app.vue组件中的primary button
    </mt-button>
  ```


#### Mint UI 中Toast组件的使用
> 消息提示框，支持自定义位置、持续时间和样式，可设置提示图标，该图标需要自己在main.js引入，例如`import 'bootstrap/dist/css/bootstrap.css'`
- 组件内按需引入，则需要`import { Toast } from 'mint-ui';`
- 执行Toast方法会返回一个toast实例，每个实例都有close方法，可用于手动关闭Toast

```html
<script>
//组件app.vue中的script
import { Toast } from "mint-ui";
export default {
  data() {
    return {
      msg: "这是组件内data函数return的对象数据",
      toastInstanse: null
    };
  },
  created() {
    //生命周期函数，组件加载完毕获取数据列表
    this.getList();
  },
  methods: {
    getList() {
      //模拟数据获取，开始就显示正在加载数据的toast
      this.show();
      setTimeout(() => {
        //使用箭头函数，使内部this指向上下文的对象的this指向
        this.toastInstanse.close();
      }, 3000);
    },
    show() {
      console.log("调用了.vue组件的show方法");
      this.toastInstanse = Toast({
        message: "提示",
        position: "top",
        duration: -1,
        iconClass: "glyphicon glyphicon-ok"
      });
    }
  }
};
</script>
```