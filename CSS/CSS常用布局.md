### CSS常用布局

#### 上中下一栏式

```html
<body>
	<!-- 上中下一栏，wrap用于统一宽度和实现居中效果-->
    <header class="wrap">#header</header>
    <section class="wrap">#sectionn</section>
    <footer class="wrap">#footer</footer>
</body>
```

注意：以下颜色值的写法是不兼容IE的



#### 左右两栏式

- 混合浮动+普通流

```html
<style type="text/css">
    .wrap {
     width: 80%;
     margin: 0 auto;
    }
    
    #left{
     width: 200px;
     height: 700px;
    float: left;
    background: #A8FAD1FF;
    }
    /* 不用给右边的元素添加固定宽度，而是使用margin-left */
    #right{
     height: 900px;
     margin-left: 200px;
     background: #F3E89AFF;
    }
 </style>

<body>
	<!-- 左右两栏式 混合浮动+普通流：让右边的内容跟随父级的宽度变化而变化 -->
	<div class="wrap">
		<aside id="left">#left</aside>
		<section id="right">#right</section>
	</div>
</body>
```



- 纯浮动式（左右两栏都左浮动，但浮动元素无法撑开父级高度，详细解决方案见fill-pit）

```html
 <style type="text/css">
    .wrap {
     width: 900px;
     margin: 0 auto;
     overflow:hidden;  /* 父级：达到撑开父级高度的效果 */
    }
 </style>
```



- 定位

```html
<style type="text/css">
    .wrap {
         width: 900px;
         margin: 0 auto;
         position: relative; /* 要将父级设置为相对定位 */
    }
    
    #left{
        width: 200px;
       height: 500px;
       background: #A8FAD1FF;
       position: absolute;  /* 将子元素设置为绝对定位 */
       left: 0; /* 右边设置为 right: 0 */
       top: 0; 
    }
</style>
```



- 左右两栏加页眉页脚——上中下+左右两栏
  先按照上中下三栏布局，在将中间部分拆分为左右两栏



#### 左中右三栏（左右两栏固定，中间根据屏幕自适应）

- 左右两边浮动，中间栏设置左右margin且将父级宽度设置为百分比

```html
<style type="text/css">
    .wrap {
        width: 80%;  /* 实现元素宽度随设备宽度的变化而变化 */
        margin: 0 auto;
    }
    #section {
        height: 500px;
    }
    #left{
        width: 200px;
        height: 100%;
        background: #A8FAD1FF;
        float: left;
    }
    
    #right {
        width: 200px;
        height: 100%;
        background: #F3E89AFF;
        float: right;
    }
    
    #main{
        margin: 0 210px;
        height: 100%;
        background: #0094FFFF;
    }
</style>
<body>
	<header id="header" class="wrap"></header>
	<section id="section" class="wrap">
		<aside id="left">#left</aside>
		<aside id="right">#right</aside>
		<section id="main">#main</section><!-- 将main放在左右栏的后面 -->
	</section>
	<footer id="footer" class="wrap"></footer>
</body>
```

- 左中右三栏之双飞翼布局

```html
<style type="text/css">
    .wrap {
        width: 80%;  /* 实现元素宽度随设备宽度的变化而变化 */
        margin: 0 auto;
    }
    
    #header {
        height: 200px;
        background: #8EFFFFFF;
    }
    
    #section {
        height: 500px;
    }
    
    #footer {
        height: 100px;
        background: #9AF070FF;
    }
    
    #main{
        width: 100%;
        height: 100%;
        background: #0094FFFF;
        float: left;
    }
    
    #left{
        width: 200px;
        height: 100%;
        background: #A8FAD1FF;
        float: left;
        margin-left: -100%;  /* 让左边栏移动到main上的左边 */
    }
    
    #right {
        width: 200px;
        height: 100%;
        background: #F3E89AFF;
        float: left;
        margin-left: -200Px; /* 让右边栏移动到main上的右边 */
    }
    
    .content{
        margin: 0 200px;  /* 让中间内容自适应 */
        height: 100%;
    }
</style>
<body>
	<header id="header" class="wrap"></header>
	<section id="section" class="wrap">
		<section id="main">
              <div class="content">#main</div><!-- main放在左右两栏上面 -->
		</section> 
		<aside id="left">#left</aside>
		<aside id="right">#right</aside>
	</section>
	<footer id="footer" class="wrap"></footer>
</body>
```

