### CSS进阶

#### CSS用户界面样式

**鼠标样式cursor**

- cursor :  default  小白 | pointer  小手  | move  移动  |  text  文本
- pointer ie6以上都支持



**轮廓 outline**

-  是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用
- 常用设置是去掉： outline: 0;   或者  outline: none;

outline : outline-color ||outline-style || outline-width 



**防止拖拽文本域resize——** resize：none 

`<textarea  style="resize: none;"></textarea> `



**vertical-align 垂直**

- vertical-align 不影响块级元素中的内容对齐，它只针对于 行内元素或者行内块元素，特别是行内块元素， 通常用来控制图片/表单与文字的对齐

`vertical-align : baseline |top |middle |bottom  `

1. 图片、表单和文字对齐（默认图片和文字基线对齐）
2. 去除图片底侧的空白缝隙（**图片或者表单等行内块元素，他的底线会和父级盒子的基线对齐**）

- 给img设置 vertical-align:middle | top等等。  让图片不要和基线对齐
- 给img 添加 display：block; 转换为块级元素就不会存在问题了



#### 溢出的文字隐藏

**word-break:自动换行（通常处理英文）**

- normal   使用浏览器默认的换行规则
- break-all   允许在单词内换行

- keep-all    只能在半角空格或连字符处换行



**white-space：通常使用于强制一行显示内容**

- normal : 　默认处理方式
- nowrap : 　强制在同一行内显示所有文本，直到文本结束或者遭遇br标签对象才换行



**text-overflow：文字溢出（设置是否使用省略符号标示文本溢出）**

- clip：不显示省略标记，只是简单裁切
- ellipsis：当对象内文本溢出时显示省略标记（···）
- 注意：使用时首先强制一行内显示nowarp，再与overflow属性搭配使用



#### 滑动门

> 为了使各种特殊形状的背景能够自适应元素中文本内容的多少，出现了CSS滑动门技术。它从新的角度构建页面，使各种特殊形状的背景能够自由拉伸滑动，以适应元素内部的文本内容，可用性更强。 最常见于各种导航栏的滑动门



**核心技术——** CSS精灵（主要是背景位置）和盒子padding

- 一般的经典布局都是这样的：

```html
<li>
  <a href="#">
    <span>导航栏内容</span>
  </a>
</li>
```

1. a 设置 背景左侧，padding撑开合适宽度。（margin-left、padding-left）    
2. span 设置背景右侧， padding撑开合适宽度 剩下由文字继续撑开宽度（padding-right）
3. 之所以a包含span就是因为 整个导航都是可以点击的。



**before和after伪元素**

- html没有对应的before和after元素，但是其所有用法和表现行为与真正的页面元素一样，可以对其使用诸如页面元素一样的css样式，表面上看上去貌似是页面的某些元素来展现，实际上是css样式展现的行为，因此被称为伪元素。

- 注意：伪元素:before和:after添加的内容默认是inline元素；这个两个伪元素的`content`属性，表示伪元素的内容,设置:before和:after时必须设置其`content`属性，否则伪元素就不起作用



**过渡（CSS3）**

- 过渡（transition）：我们可以在不使用 Flash 动画或 JavaScript 的情况下，当元素从一种样式变换为另一种样式时为元素添加效果`transition: 要过渡的属性  花费时间（单位是s） 运动曲线（默认是ease） 何时开始;`

- 帧动画：通过一帧一帧的画面按照固定顺序和速度播放

```css
/*如果有多组属性变化，还是用逗号隔开。*/
div {
    width: 200px;
    height: 100px;
    background-color: pink;
    /* transition: 要过渡的属性  花费时间  运动曲线  何时开始; */
    transition: width 0.6s ease 0s, height 0.3s ease-in 1s;
    /* transtion 过渡的意思  这句话写到div里面而不是 hover里面 */
}

div:hover {  /* 鼠标经过盒子，我们的宽度变为400 */
    width: 600px;
    height: 300px
}

transition: all 0.6s;  /* 所有属性都变化用all  后面俩个属性可以省略 */
```



#### 2D变形（CSS3）transform

**移动 transdform:translate(x, y)**

- 使用translate方法来将文字或图像在水平方向和垂直方向上分别垂直移动x、y像素
-  translateX(x)仅水平方向移动、translateY(Y)仅垂直方向移动

```css
/* 让定位的盒子水平居中*/
.box {
  width: 499.9999px;
  height: 400px;
  background: pink;
  position: absolute;
  left:50%;
  top:50%;
  transform:translate(-50%,-50%);  /* 移动自己的一半 */
}
```



**缩放 transform:scale(x, y)** 

`transform:scale(0.8,1); /* 使该元素在水平方向上缩小了20%，垂直方向上不缩放 */ `

- scale()的取值默认的值为1，当值设置为0.01到0.99之间的任何值，作用使一个元素缩小；而任何大于或等于1.01的值，作用是让元素放大



**旋转transform:rotate(deg)**

- 可以对元素进行旋转，正值为顺时针，负值为逆时针；

`transform:rotate(45deg);  /deg是度数/ `



**transform-origin 可以调整元素转换变形的原点**

-  如果是4个角，可以用 left top这些，如果想要精确的位置， 可以用  px 像素。

```css
 /* 改变元素原点到左上角，然后进行顺时旋转45度 */ 
 div{transform-origin: left top;transform: rotate(45deg); } 
 
 /* 改变元素原点到x 为10  y 为10，然后进行顺时旋转45度 */ 
  div{transform-origin: 10px 10px;transform: rotate(45deg); }
```



**倾斜：transform:skew(deg,deg)**

- 可以使元素按一定的角度进行倾斜，可为负值，第二个参数不写默认为0

`transform:skew(30deg,0deg);/* 水平方向上倾斜30度，垂直方向保持不变 */`



#### 3D变形(CSS3) transform

—— 右手法则：x左负右正、y下负上正、z里负外正

**transform:rotateX()——沿X轴立体旋转**

```css
img {
  transition:all 0.5s ease 0s;
}
img:hover {
  transform:rotateX(180deg);/*rotateY()、rotateZ()同理沿Y轴Z轴旋转*/
}
```



**透视(perspective)**

- 透视原理： 近大远小 。
- 浏览器透视：把近大远小的所有图像，透视在屏幕上。
- perspective：视距，表示视点距离屏幕的长短。视点，用于模拟透视效果时人眼的位置



 **transform:translateX(x)—— 沿X轴水平方向移动**

- 同理还有translateY(y)、translateZ(z)

- translateZ(z)

- - 实质是XY平面相对于视点的远近变化（说远近就一定会说到离什么参照物远或近，在这里参照物就是perspective属性）。比如设置了perspective为200px;那么transformZ的值越接近200，就是离的越近，看上去也就越大，超过200就看不到了，因为相当于跑到后脑勺去了，我相信你正常情况下，是看不到自己的后脑勺的。



**transform:translate3d(x,y,z)**

- 其中，x和y可以是长度值，也可以是百分比，百分比是相对于其本身元素水平方向的宽度和垂直方向的高度；z只能设置长度值



**backface-visibility**

- backface-visibility 属性定义当元素不面向屏幕时是否可见(旋转180度时)

```css
div {
    width: 224px;
    height: 224px;
    margin: 100px auto;
    position: relative;
}

div img {
    position: absolute;
    top: 0;
   left: 0;
   transition: all 1s; 
}

div img:first-child {
    z-index: 1;
    backface-visibility: hidden; /* 不是正面对象屏幕，就隐藏 */
}

div:hover img {
    transform: rotateY(180deg);
}
```



**动画(CSS3) animation**

—— 可通过设置多个节点来精确控制一个或一组动画，常用来实现复杂的动画效果

animation:动画名称 动画时间 运动曲线  何时开始  播放次数  是否反方向; 

| 属性                      | 描述                                                 |
| ------------------------- | ---------------------------------------------------- |
| @keyframes                | 规定动画                                             |
| animation                 | 所有动画属性的简写属性，除了animation-paly-state属性 |
| animation-duration        | 规定动画完成一个周期所花费的秒或毫秒。默认是0        |
| animation-timing-function | 规定动画的速度曲线，默认是ease                       |
| animation-delay           | 规定动画何时开始，默认是0                            |
| animation-iteration-count | 规定动画被播放的次数，默认是1                        |
| animation-direction       | 规定动画是否在下一周期逆向播放，默认是normal         |
| animation-play-state      | 规定动画是否正在运行或暂停，默认是“running”          |
| animation-fill-mode       | 规定对象动画时间之外的状态                           |



注意：除了名字、动画时间、延时有严格的顺序要求其他的随意

```
/*
@keyframes 动画名称 {
  from{ 开始位置 } == 0%
  to{  结束  } == 100%
}
    animation-iteration-count:infinite;  无限循环播放
    animation-play-state:paused;   暂停动画"
*/

/* 小汽车案例 */
body {
  background: white;
}
img {
  width: 200px;
}
.animation {
  animation-name: goback;
  animation-duration: 5s;
  animation-timing-function: ease;
  animation-iteration-count: infinite;
}
@keyframes goback {
  0%{}
  49%{
    transform: translateX(1000px);
  }
  55%{
    transform: translateX(1000px) rotateY(180deg);
  }
  95%{
    transform: translateX(0) rotateY(180deg);
  }
  100%{
    transform: translateX(0) rotateY(0deg);
  }
}
```



**伸缩布局（CSS3）**

- 主轴：Flex容器的主轴主要用来配置Flex项目，默认是水平方向

-  侧轴：与主轴垂直的轴称作侧轴，默认是垂直方向的

-  方向：默认主轴从左向右，侧轴默认从上到下

- 主轴和侧轴并不是固定不变的，通过flex-direction可以互换

（语法规范版本众多，浏览器支持不一致，致使Flexbox布局使用不多）



>  flex子项目在主轴的缩放比例，不指定flex属性，则不参与伸缩分配

```css
min-width  280px;     /* 最小宽度  不能小于 280 */
max-width: 1280px;  /*最大宽度  不能大于 1280/
```



>  flex-direction 调整主轴的方向（默认是水平方向）

```css
flex-direction: column 垂直排列
flex-direction: row  水平排列

http://m.ctrip.com/html5/   携程网手机端地址
```



> justify-content调整主轴对齐（水平对齐）

| 值            | 描述                                             | 白话文                                         |
| ------------- | ------------------------------------------------ | ---------------------------------------------- |
| flex-start    | 默认值。项目位于容器的开头。                     | 让子元素从父容器的开头开始排序但是盒子顺序不变 |
| flex-end      | 项目位于容器的结尾。                             | 让子元素从父容器的后面开始排序但是盒子顺序不变 |
| center        | 项目位于容器的中心。                             | 让子元素在父容器中间显示                       |
| space-between | 项目位于各行之间留有空白的容器内。               | 左右的盒子贴近父盒子，中间的平均分布空白间距   |
| space-around  | 项目位于各行之前、之间、之后都留有空白的容器内。 | 相当于给每个盒子添加了左右margin外边距         |



> align-items调整侧轴对齐（垂直对齐）

| 值         | 描述                           | 白话文                                                |
| ---------- | ------------------------------ | ----------------------------------------------------- |
| stretch    | 默认值。项目被拉伸以适应容器。 | 让子元素的高度拉伸适用父容器（子元素不给高度的前提下) |
| center     | 项目位于容器的中心。           | 垂直居中                                              |
| flex-start | 项目位于容器的开头。           | 垂直对齐开始位置 上对齐                               |
| flex-end   | 项目位于容器的结尾。           | 垂直对齐结束位置 底对齐                               |



> flex-wrap控制是否换行

—— 当我们子盒子内容宽度多于父盒子的时候如何处理

| 值           | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| nowrap       | 默认值。规定灵活的项目不拆行或不拆列。 不换行，则 收缩（压缩） 显示 强制一行内显示 |
| wrap         | 规定灵活的项目在必要的时候拆行或拆列。                       |
| wrap-reverse | 规定灵活的项目在必要的时候拆行或拆列，但是以相反的顺序。     |



> flex-flow是flex-direction、flex-wrap的简写形式

```css
/*flex-flow: flex-direction(排列方向)  flex-wrap（是否换行）;  */

display: flex;
/* flex-direction: row; 
flex-wrap: wrap;   这两句话等价于下面的这句话*/
flex-flow: column wrap;  /* 两者的综合 */
```



> align-content堆栈（由flex-wrap产生的独立行）多行垂直对齐方式

- align-content是针对flex容器里面多轴(多行)的情况,align-items是针对一行的情况进行排列。

- 必须对父元素设置自由盒属性display:flex;，并且设置排列方式为横向排列flex-direction:row;并且设置换行，flex-wrap:wrap;这样这个属性的设置才会起作用。

| 值            | 描述                                             |
| ------------- | ------------------------------------------------ |
| stretch       | 默认值。项目被拉伸以适应容器。                   |
| center        | 项目位于容器的中心。                             |
| flex-start    | 项目位于容器的开头。                             |
| flex-end      | 项目位于容器的结尾。                             |
| space-between | 项目位于各行之间留有空白的容器内。               |
| space-around  | 项目位于各行之前、之间、之后都留有空白的容器内。 |



> order控制子项目的排列顺序，正序方式排序，从小到大

- 用整数值来定义排列顺序，数值小的排在前面。可以为负值。 默认值是 0

`order: 1; `