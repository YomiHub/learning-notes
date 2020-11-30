### jQuery基础

参考书：《锋利的jQuery》 

jQuery官网：http://jquery.com 

jQuery在线API：http://api.jquery.com   http://api.jquery.com/api/ 

jQuery UI：http://jqueryui.com/



#### jQuery和Dom对象互转

**dom转成jQuery**

- 如何将dom对象转成jquery对象,加$就可以了

`$(dom对象)`

- 将jquery对象转dom对象

`$("#btn")[0]和$("#btn").get(0)`都可以转dom对象 

```js
$(function () { 
    var dv1 = $("#dv");
    var dv2 = document.getElementById("dv");// dom对象 
    // 必然报错 , 为什么 ? 
    //因为 jquery 对象不能直接调用 dom 中对象的属性或者是方法 
    dv1.innerHTML = "<p>哈哈</p>"; 
    
    // 同理 , 下面这行代码必然报错 , 为什么呢 ? 
    //因为 dom 对象是不能直接调用 jquery 对象的方法 的 
    dv2.html("<p>哈哈</p>"); 
});
```



#### jQuery选择器(重点)

**选择器的使用**

- 常用：id选择器,标签选择器,类选择器 

- 标签加类选择器 $(li.cls) 

- 多条件选择器 $(span,li,div)

- 层级选择器

- - 后代选择器$(“#dv li”)或者$(“ul li”)
  - 子代选择器(直接的所有子元素,儿子) $(“#ul>li”)
  - 获取当前元素的相邻元素：$(“div + span”) 
  - 获取当前元素后面所有元素 ：$(“div ~ span”)

- 常见的选择器,(可以理解成是过滤器) 

- - :even偶数 选择器   $("#u1 li:even")
  - :odd奇数 选择器

- 索引选择器 

- - eq(3)获取索引的元素 
  - gt(3)索引大于3的所有元素 
  - lt(3)索引小于3的所有的元素 



**获得兄弟元素**

- next(); //当前元素之后的紧邻着的第一个兄弟元素（下一个） 
- nextAll();//当前元素之后的所有兄弟元素 
- prev();//当前元素之前的紧邻着的兄弟元素（上一个） 
- prevAll();//当前元素之前的所有兄弟元素
- siblings();//当前元素的所有兄弟元素



#### jQuery常见的方法

**.html()方法**

- 获取和设置标签中间显示其他标签及内容,类似于 innerHTML ，设置内容后 , 原内容丢失 , 返回的是当前对象
- .html方法可以创建元素      $(“html标签”)也可以创建元素



**.text()方法**

- 获取和设置标签中间显示的文本内容,类似于 innerText 



**.val()方法**

- 获取和设置input标签中value的值,类似于value 



**attr()方法  |  prop()方法**

- 可以获取或设置属性值,两个参数:1.属性名字；参数2,属性值 



**.css()方法**

- 设置元素的样式,类似于style（写的是键值对）

```js
// 第一种写法 :
 $("#u1>li").css("backgroundColor","red"); 
 $("#u1>li").css("fontSize","全新硬笔行书简"); 
 $("#u1>li").css("fontSize","30px"); 
 
 // 第二种写法 : 
 var style={ 
     "backgroundColor":"red", 
     "fontSize":"全新硬笔行书简",
      "fontSize":"30px" 
}; 
 $("#u1>li").css(style); 
 
 // 第三种写法 :
  $("#u1>li").css("backgroundColor","red").css("fontSize","全新硬笔行书简 ").css("fontSize","30px");
```



**位置操作**

- Offset()方法返回的是对象
  - 设置的时候元素在设置前如果没有脱离文档流,设置的时候会自 动进行脱离文档流,默认为relative 

```js
// 设置前如果有脱离文档流 , 设置的时候不会再次设置脱标
console.log($("div").offset().left);
$("div").offset({"left":100,"top":100});
```



- scrollLeft()和scrollTop

```js
$(function () { 
    $(window).scroll(function () {
     var h=$(".top").height(); 
     var top=$(window).scrollTop(); 
     if(top>=h){ 
         $(".nav").css({"position":"fixed","top":0}); 
         // 此时 main 为固定定位，所以设置 margin-top 
         $(".main").css({"marginTop":$(".nav").height()}); 
     }else{
         $(".nav").css({"position":"static"}); 
         // 此时 main 为固定定位，所以设置 margin-top 
         $(".main").css({"marginTop":0}); 
     } 
     }); 
});
```



#### 动画

**动画常用方法**

- hide()、show()
- slideUp()滑入、slideDown()滑出、.slideToggle()切换滑入滑出
- fadeIn()、fadeOut()、fadeToggle()
- fadeTo()  // 可以设置透明度 , 参数 1: 时间 , 参数 2: 到达透明度 



**动画方法—— animate()**

- 第一个参数:键值对,(数值的属性可以改,颜色不能改) 
- 第二个参数:动画的时间 
- 第三个参数:回调函数

```js
$("#im").animate({"left":"10px","top":"500px","width": "50px", "height": "50px"},2000,function () {
     $("#im").animate({"left":"+=505px","top":"-=400px","width":"+=200px","height":"+=200px"
     },1000);
});

```



**停止动画效果:stop()方法,**

- 解决下拉框案例中的bug

```js
$(".wrap>ul>li").mouseover(function () { 
    $(this).children("ul").stop().show(500); // 显示 ul
});
 $(".wrap>ul>li").mouseout(function () { 
     $(this).children("ul").stop().hide(500);// 显示 ul 
 });
```



#### jQuery操作类样式 

**addClass():**

- 参数:类样式名字，添加样式的同时不 会干掉原有的样式 



**removeClass():**

- 不写参数移除所有的类样式 



**removeClass(“cls“)**

- 移除指定的一个类样式



**jQuery创建元素及追加元素**

- append、prepend
  - append用来在元素的末尾追加元素（最后一个子节点）。增加元素末尾(儿 子) 
  - prepend  在元素的开始添加元素（第一个子节点）。增加元素开始(儿子) 



- after、before
  - after  在元素之后添加元素（添加兄弟）增加元素后面(兄弟)
  - before  在元素之前添加元素（添加兄弟）增加元素前面(兄弟)



#### 绑定事件

- bind()
  - bind方法可以为动态创建元素绑定事件,但是不怎么用



- delegate()
  - delegate()方法为父级元素绑定事件,子元素来触发

```js
$("#btn").bind("click",function () { 
    $("div").append($("<p>创建后的p</p>")); 
    // 参数 1: 要触发事件的动态元素
     // 参数 2: 要触发的事件名字 ( 事件的类型 ) 
     // 参数 3: 执行事件的函数
     $("div").delegate("p","click",function () { alert("哎呀"); }); 
});

```



- 绑定事件on

```js
/* 
* 参数 1: 绑定事件的类型 
* 参数 2: 绑定事件的元素 
* 参数 3: 执行事件的函数 
* */
 $("div").on("click","p",function () { alert("哎呀"); });
```



给动态元素绑定事件 —— 推荐使用的绑定方法

- 很现代的方式 节约内存，效率高，时间少

- - 解决动态创建表格添加元素绑定事件bug

```js
$("#j_tb").on("click",".get",function () { 
    // "#j_tb"下".get"的点击事件
    $(this).parent("td").parent("tr").remove();
});
```



- off()解绑事件
  - 推荐:用什么方式绑定事件就要用对应的解绑事件解除绑定 
  - 解除绑定事件:自身的绑定事件和动态绑定的事件都会被解绑

```js
$("#btn").bind("click",function(){}); 
$("#btn").unbind("click"); 

$("#dv").delegate("p","click",function(){}); 
$("#dv").undelegate("p","click");

// 解绑多个事件 
$("#btn1").off("click mouseover");

```



```js

$("div").on("click",function () { 
    alert("层被点了");
 }); 
 
 $("div").on("click","p",function () {
    alert("p被点了");
}); 

$("#btn2").on("click",function () { 
 // 解绑事件的时候 div 和 p 同时解绑 ( 所有的 )
 // 自身绑定的事件以及动态绑定的事件都会解绑   $("div").off()
 $("div").off("click"); 
 });

```



#### 触发事件

**click()**

- 第一种方式:直接调用元素的事件方法: 

```js
$("div").click(); 
```



- 第二种方式:使用.trigger()方法,扳机

```js
$("div").trigger("click");
```



- 第三种方式:使用.triggerHandle()方法

```js
$("div").triggerHandler("click");
```

注意：.trigger()和.triggerHandler()区别 前者会触发浏览器的默认行为,并执行事件 后者不会触发浏览器默认行为,但是会执行 事



**事件对象**

- event.delegateTarget:代码绑定事件的对象 
- event.currentTarget:绑定事件的对象 
- event.target:真正触发事件的对

```js
$("div").on("click",function (event) { console.log(event); }); 
```



**取消事件冒泡+取消默认事**

- 取消事件冒泡: e.stoPropagation() 
- 取消默认事件： event.preventDefault(); 
- return false 不仅可以用来取消事件冒泡 同样可以取消默认事件

```js
$("a").on("click",function () { 
    alert("超链接"); 
    return false;
});
```



**链式编程——评分案例**

```js
$(".comment>li").on("mouseover",function () { 
 $(this).text("★").prevAll("li").text("★").end().nextAll("li").text("☆");
}).on("mouseout",function () { 

  $(this).parent().children("li").text("☆"); 
  // 第二步 :2
  $(".comment>li[index=1]").text("★").prevAll("li").text("★").end().nextAll("li").text("☆"); 
  }).on("click",function () { 
  // 第二步 :1
   $(this).attr("index",1).siblings("li").removeAttr("index");
});
```

//   对于end()方法，jQuery文档是这样解释的：jQuery回到最近的一个"破坏性"操作 之前。即: 将匹配的元素列表变为前一次的状态。



**遍历 each()**

- jQuery中有隐式迭代,不需要我们再次进行遍历对某些元素进行操作 但是,如果涉及到对不同的元素有不同的操作,那么需要进行each遍历

```js
// 参数：表示每次遍历都要执行的函数 
$("li").each(function (index, ele) { 
// index 表示：当前这个元素的索引号 从 0 开始的 0 - 10
    var op = (index + 1) / 10; 
    $(ele).css("opacity", op);
});
```



**多库共存**

如果此时其他的库文件中也使用了$符号 ——解决命名空间的冲突 

- :$.noConflict()

```js
// 让 jQuery 释放对 $ 的控制权
 $.noConflict(); 
 var $="哦买噶"; 
 console.log($); 
 jQuery(function () {
     jQuery("#btn").click(function () { 
         alert("正常的执行"); 
      }); 
})

var itcast=$.noConflict();    //命名
var $="哦买噶"; 
console.log($); 
itcast(function () {
    itcast("#btn").click(function () { 
       alert("正常的执行"); 
    }); 
});
```



**jQuery的迭代**

-  $(“#id”).length>0  用途：

- - 可以判断页面上的某个元素是否存在， 如果存在则不再重新创建，如果不存在则创建一个新的并显示 （动态创建元素的时候用。） 

```js
 if ($("#btn1").length <= 0) { 
   alert("id为btn1的元素不存在！"); 
}
```



#### jqueryUI

**jQueryUI的使用**

1.引入jQueryUI的样式文件 

2.引入jQuery 

3.引入jQueryUI的js文件 

4.使用jQueryUI功能



- 如何判断对象是否存在，jQuery选择器返回的是一个对象数组
  - 调用text()、html()、click()之类方法的时候其实是对数组中每个元素迭代调用每个方法，因此即使通过id选择的元素不存在也不会报错，如果需要判断指定的id是否存在，应该写：

```js
if ($("#btn1").length <= 0) {   //如果不存在就创建一个新的
 alert("id为btn1的元素不存在！");
}
```



- 不论鼠标指针穿过被选元素或其子元素，都会触发 mouseover 事件。

- 只有在鼠标指针穿过被选元素时，才会触发 mouseenter 事件