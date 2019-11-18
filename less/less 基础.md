### less 基础



**使用less**

https://less.bootcss.com/# 找到less，下载less.js (js支持所有现代浏览器(Chrome、Firefox、Safari、IE11+和Edge的最新版本))



#### less是什么

- （官方）一种动态 样式 语言

- - 变量、继承、运算、函数

- 一个写CSS的工具

- - 更灵活的统筹全局
  - 更方便计算



#### 使用less

**在客户端使用less，直接引入less.js**

—— 备注：请在服务器环境下使用！本地直接打开可能会报错https://less.bootcss.com/#

```html
在引入less.js前先要把你的样式文件引入 :
<link rel="stylesheet/less" type="text/css" href="styles.less">
<script src="//cdnjs.cloudflare.com/ajax/libs/less.js/3.10.0-beta/less.min.js" ></script>
```

- 好处：能获取到客户端的数据，从而进一步计算
- 坏处：直接在客户端解析Less，造成性能浪费，不利于维护



**koala编译器**

- 每次都要打开软件
- 替代方案：VScode扩展插件使less文件实时生成css/wxss代码



**在服务器端使用**

```
在服务器端安装 LESS 的最简单方式就是通过 npm(node 的包管理器), 像这样:
npm install -g less

如果你想下载最新稳定版本的 LESS，可以尝试像下面这样操作:
$ npm install less@latest

就可以在Node中像这样调用编译器:
var less = require('less');

less.render('.class { width: 1 + 1 }', function (e, css) {
    console.log(css);
});
```



**在命令行下使用**

```
你可以在终端调用 LESS 解析器:
$ lessc styles.less

上面的命令会将编译后的 CSS 传递给 stdout, 你可以将它保存到一个文件中:
$ lessc styles.less styles.css

如何你想将编译后的 CSS 压缩掉，那么加一个 -x 参数就可以了.

案例：E:
    cd 0.AllProjectByYan\VScodeProject\lessTest
    lessc center.less center.css
```



**在编辑器中实时编译（推荐使用）**

```json
在VScode中安装扩展Easy less
在扩展中搜索Easy less并安装，文件在保存的时候会生成css并实时更新

Ctrl+ 逗号 打开设置 - 搜索 easy Less config 并选择 点在setings.json中编辑 把下面配置粘贴进去（注意配置项间隔的逗号不要漏了）
"less.compile": {
    // "compress":  true,  // 是否删除多余空白字符
    // "sourceMap": true,  // 是否创建文件目录树，true的话会自动生成一个 .css.map 文件
    "outExt": ".wxss" // 输出文件的后缀,默认为.css
}
```

**less中的单行注释不会被编译到css文件中，多行注释会被编译到css文件中**



**变量**

- 变量允许单独定义一系列通用的样式，然后在需要的时候去调用，所以在做全局样式调整的时候我们可能只需要修改几行代码即可。
- @rem  （@变量名来定义变量）
- 作用域 （全局声明和局部声明）

```less
@height:`document.documentElement.clientHeight`;  //在软件中编译无法识别

div{
    @width：100px;
    width:@width;
    height:@height;
}
```



**嵌套**

- 可以在一个选择器中嵌套另一个选择器来实现继承，这样可以很大程度减少了代码量，并且代码看起来更加的清晰
- & 在嵌套里面代表元素本身（解决嵌套里面只放子元素的弊端）

```less
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
      width: 300px;
  }
  a{
     color:#fff;
     &:hover{
         color:#blue;
     }
  }
}
```



**混合**

- 将一个定义好的clasA引入到另一个classB 中，从而简单实现classB 继承classA中所有属性

- - 带参数混合（参数设置默认值）

```less
//第一种： 最简单的混合
.border{
    border:1px solid #000;
}

div{
    .border;   //继承.border中所有的样式
}

//第二种：带参数混合
.border(@color){
    border:1px solid @color;
}

//第三种：多个参数的混合
.border(@width,@color){
    border:@width solid @color;
}

div{
    .border(1px,red)
}

//带默认值参数混合
.border(@width:1px){   //设置默认值，不传参时取默认值
    border:@width solid @color;
}
```



- @arguments变量（所有变量）

- - 传一个参数时，则为第一个，其余保持默认值
  - 注意传参时顺序保持一致

```less
.border(@width:1px，@style:solid){   //设置默认值，不传参时取默认值
    border:@arguments @color;
}
```



- 模式匹配

- - 首个参数可以随意命名，以匹配不同的样式

```less
.border(@_,@width:1px){
    width:200px;   //公共样式，默认首参,但是调用的时候还是一定要带首参
}

.border(top,@width:1px){
    border-top:@width solid #0333333;
}
.border(left,@width:1px){
    border-top:@width solid #0333333;
}

div{
    .border(top);
}
```



- 雪碧图



**其他**

- Math函数

- - round(1.67);    // 四舍五入 2   例：round(1.67)*1px   添加单位px
  - ceil(2.4)     //向上取整  3
  - floor(2.6)   //向下取整  2



- 命名空间 —— 混合

```less
.blue{
    .button{
        background:blue;
    }
}

.red{
    .button{
        background:red
    }
}

div{
    .red > .button;   //带上尖括号
}
```



- 注释（单行和多行，区别在于是否会被编译到css文件）
- importing  —— @import "lib.less"    避免html多次引用css文件

```less
@import "reset.less"  //.less后缀可加可不加
```



- 避免编译 —— ~

```less
font:(12/@rem)~'/'(20/@rem) '宋体'

css下：
font:0.375rem/0.625rem '宋体'   //自带的‘/’表示字体大小与行高，不希望进行计算
```

