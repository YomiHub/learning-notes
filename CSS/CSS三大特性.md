### CSS三大特性

层叠、继承、优先级

#### CSS层叠性
--------------------------------------------------------------------------------

> 是浏览器处理冲突的一个能力,如果一个属性通过两个相同选择器设置到同一个元素上，那么这个时候一个属性就会将另一个属性层叠掉

- 出现样式冲突时，会按照CSS书写的顺序，以最后的样式为准



#### CSS继承性
--------------------------------------------------------------------------------

> 所谓继承性是指书写CSS样式表时，子标签会继承父标签的某些样式，如文本颜色和字号

- 恰当地使用继承可以简化代码，降低CSS样式的复杂性。子元素可以继承父元素的样式（text-，font-，line-这些元素开头的都可以继承，以及color属性）




#### CSS优先级
--------------------------------------------------------------------------------

>  需要注意的一些特殊情况

- 继承样式的权重为0: 即在嵌套结构中，不管父元素样式的权重多大，被子元素继承时，他的权重都为0
- 行内样式优先: 应用style属性的元素，其行内样式的权重非常高，可以理解为远大于100
- 权重相同时，CSS遵循就近原则，换句话说排在最后的样式优先级最大。
- CSS定义了一个!important命令，该命令被赋予最大的优先级



> CSS特殊性

- 关于CSS权重，我们需要一套计算公式来去计算，这个就是 CSS Specificity，我们称为CSS 特性或称非凡性，它是一个衡量CSS值优先级的一个标准
  - specificity用一个四位的数 字串(CSS2是三位)来表示，更像四个级别，值从左到右，左面的最大，一级大于一级，数位之间没有进制，级别之间不可超越
    - 继承或者* 的贡献值 0,0,0,0
    - 每个元素（标签）贡献值为 0,0,0,1
    - 每个类，伪类贡献值为 0,0,1,0
    - 每个ID贡献值为 0,1,0,0
    - 每个行内样式贡献值 1,0,0,0
    - 每个!important贡献值 ∞ 无穷大



注意：权重是可以叠加的（继承的 权重是 0）

```
div ul  li   ------>      0,0,0,3

.nav ul li   ------>      0,0,1,2

a:hover      -----—>      0,0,1,1

.nav a       ------>      0,0,1,1   

#nav p       ----->       0,1,0,1
```



重要：数位之间**没有进制**比如说： 0,0,0,5 + 0,0,0,5 =0,0,0,10 而不是 0,0, 1, 0， 所以不会存在10个div能赶上一个类选择器的情况。



#### 总结优先级：
1. 使用了 !important声明的规则。
2. 内嵌在 HTML 元素的 style属性里面的声明。
3. 使用了 ID 选择器的规则。
4. 使用了类选择器、属性选择器、伪元素和伪类选择器的规则。
5. 使用了元素选择器的规则。
6. 只包含一个通用选择器的规则。
7. 同一类选择器则遵循就近原则。

**权重是优先级的算法，层叠是优先级的表现**