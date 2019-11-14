#### BFC+CSS验证压缩

**BFC（块级格式化上下文）**

> 一个块格式化上下文（block formatting context） 是Web页面的可视化CSS渲染出的一部分。它是块级盒布局出现的区域，也是浮动层元素（脱离文档流）进行交互的区域



**元素的显示模式**

- 元素的显示模式 display：分为 块级元素   行内元素  行内块元素 ，其实，它还有很多其他显示模式—— table-cell、compact、list-item、table-row··



**哪些元素会具有BFC的条件**

- display 属性为 block, list-item, table 的元素，会产生BFC
- 注意其他的，display属性，比如 line 等等，他们创建的是 IFC 



**什么情况下可以让元素产生BFC**

- 以上盒子具有BFC条件了，就是说有资质了，但是怎样触发才会产生BFC，从而创造这个封闭的环境呢？ 
- 给这些元素添加如下属性就可以触发BFC。
  - float属性不为none
  - position为absolute或fixed
  - display为inline-block, table-cell, table-caption, flex, inline-flex
  - overflow不为visible



**BFC元素所具有的特性**

- 在BFC中，盒子从顶端开始垂直地一个接一个地排列

- 盒子垂直方向的距离由margin决定。属于同一个BFC的两个相邻盒子的margin会发生重叠

- 在BFC中，每一个盒子的左外边缘（margin-left）会触碰到容器的左边缘(border-left)（对于从右到左的格式来说，则触碰到右边缘）

  - BFC的区域不会与浮动盒子产生交集，而是紧贴浮动边缘。
  - 计算BFC的高度时，自然也会检测浮动或者定位的盒子高度



**BFC的主要用途**

- 清除元素内部浮动（清除浮动造成的影响）
  
- 只要把父元素设为BFC就可以清理子元素的浮动影响了，最常见的用法就是在父元素上设置overflow: hidden（触发BFC）样式，对于IE6加上zoom:1就可以了
  
  
  
- 解决外边距合并问题

  - 属于同一个BFC的两个相邻盒子的margin会发生重叠，那么我们创建不属于同一个BFC，就不会发生margin重叠了

  

- 制作右侧自适应的盒子问题
  - 普通流体元素BFC后，为不和浮动元素产生任何交集，顺着浮动边缘形成自己的封闭上下文



>  BFC 总结：BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。包括浮动，和外边距合并等等



#### 优雅降级和渐进增强

**渐进增强** progressive enhancement

- 针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验



**优雅降级** graceful degradation

- 一开始就构建完整的功能，然后再针对低版本浏览器进行兼容



> 个人建议： 现在互联网发展很快， 连微软公司都抛弃了ie浏览器，转而支持 edge这样的高版本浏览器，我们很多情况下没有必要再时刻想着低版本浏览器了，而是一开始就构建完整的效果，根据实际情况，修补低版本浏览器问题。



**浏览器前缀**

| 浏览器前缀 | 浏览器                                 |
| ---------- | -------------------------------------- |
| -webkit-   | Google Chrome, Safari, Android Browser |
| -moz-      | Firefox                                |
| -o-        | Opera                                  |
| -ms-       | Internet Explorer, Edge                |
| -khtml-    | Konqueror                              |

—— 后面我们会有 常用的解决H5和C3 的兼容解决文件， 我们这里暂且不涉及



**背景渐变**

> 在线性渐变过程中，颜色沿着一条直线过渡：从左侧到右侧、从右侧到左侧、从顶部到底部、从底部到顶部或着沿任何任意轴。



- 兼容性问题很严重

```css
/*线性渐变*/
background:-webkit-linear-gradient(渐变的起始位置， 起始颜色， 结束颜色)；
background:-webkit-linear-gradient(渐变的起始位置， 颜色位置， 颜色位置....)；
```



#### CSS W3C 统一验证工具

- CssStats 是一个在线的 CSS 代码分析工具:http://www.cssstats.com/
- W3C 统一验证工具(可以检测本地文件)：    http://validator.w3.org/unicorn/



**CSS压缩**

- 通过上面的检测没有错误，为了提高加载速度和节约空间（相对来说，css量很少的情况下，几乎没啥区别），可以通过css压缩工具把css进行压缩。

  - w3c css压缩   http://tool.chinaz.com/Tools/CssFormat.aspx   网速比较慢
  - 还可以去站长之家进行快速压缩：http://tool.chinaz.com/Tools/CssFormat.aspx  