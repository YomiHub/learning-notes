### Web API--DOM、BOM

#### Web API介绍

**API的概念**

- API（Application Programming Interface,应用程序编程接口）是一些预先定义的函数，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问源码，或理解内部工作机制的细节（任何开发语言都有自己的API）



**Web API的概念**

- 浏览器提供的一套操作浏览器功能和页面元素的API(BOM和DOM)
- MDN-Web API —— https://developer.mozilla.org/zh-CN/docs/Web/API



**JavaScript的组成**

- ECMAScript - JavaScript

- - 定义了JavaScript 的语法规范
  - JavaScript的核心，描述了语言的基本语法和数据类型，ECMAScript是一套标准，定义了一种语言的标准与具体实现无关



- BOM - 浏览器对象模型

- - 一套操作浏览器功能的API
  - 通过BOM可以操作浏览器窗口，比如：弹出框、控制浏览器跳转、获取分辨率等 



- DOM - 文档对象模型

- - 一套操作页面元素的API
  - DOM可以把HTML看做是文档树，通过DOM提供的API可以对树上的节点进行操作



#### DOM

**DOM的概念**

- 文档对象模型（Document Object Model，简称DOM），是[W3C](https://baike.baidu.com/item/W3C)组织推荐的处理[可扩展标记语言](https://baike.baidu.com/item/可扩展置标语言)的标准[编程接口](https://baike.baidu.com/item/编程接口)。它是一种与平台和语言无关的[应用程序接口](https://baike.baidu.com/item/应用程序接口)(API),它可以动态地访问程序和脚本，更新其内容、结构和[www](https://baike.baidu.com/item/www/109924)文档的风格
- [DOM](https://baike.baidu.com/item/DOM/50288)是一种基于树的[API](https://baike.baidu.com/item/API/10154)文档，它要求在处理过程中整个文档都表示在[存储器](https://baike.baidu.com/item/存储器)中
  - 文档：一个网页可以称为文档
  - 节点：网页中的所有内容都是节点（标签、属性、文本、注释等）
  - 元素：网页中的标签
  - 属性：标签的属性



**DOM经常进行的操作**

- 获取元素
- 对元素进行操作(设置其属性或调用其方法)
- 动态创建元素
- 事件(什么时机做相应的操作)



**获取页面元素**

如何获取页面元素——id、标签名、name、类名、选择器

- 重点掌握getElementById、getElmentsBytagName

```js
//根据Id获取
var div = document.getElementById('main');

//根据标签名获取
var divs = document.getElementsByTagName('div');
for (var i = 0; i < divs.length; i++) {
  var div = divs[i];
  console.log(div);
} 

//根据name获取
var inputs = document.getElementsByName('hobby');
for (var i = 0; i < inputs.length; i++) {
  var input = inputs[i];
  console.log(input);
}

//根据类名获取
var mains = document.getElementsByClassName('main');
for (var i = 0; i < mains.length; i++) {
  var main = mains[i];
  console.log(main);
}

//根据选择器获取
var text = document.querySelector('#text');
console.log(text);

var boxes = document.querySelectorAll('.box');
for (var i = 0; i < boxes.length; i++) {
  var box = boxes[i];
  console.log(box);
}
```



#### 事件

— — 触发-响应机制

**事件三元素**

- 事件源:触发(被)事件的元素
- 事件名称: 如click 点击事件
- 事件处理程序:事件触发后要执行的代码(函数形式)



**属性操作**

> 非表单元素的属性：href、title、id、src、className

- innerHTML（能够识别标签）和innerText（不能识别标签）
- HTML转义符

```
"		&quot;
'		&apos;
&		&amp;
<		&lt;   // less than  小于
>		&gt;   // greater than  大于
空格	   &nbsp;
©		&copy;
```

- innerHTML和innerText的区别
- innerText的兼容性处理



> 表单元素属性

- value 用于大部分表单元素的内容获取(option除外)

- type 可以获取input标签的类型(输入框或复选框等)
- disabled 禁用属性
- checked 复选框选中属性
- selected 下拉菜单选中属性



> 自定义属性操作

- getAttribute() 获取标签行内属性
- setAttribute() 设置标签行内属性
- removeAttribute() 移除标签行内属性
- 与element. 属性的区别: 上述三个方法用于获取任意的行内属性



**样式操作**

- 使用style方式设置的样式显示在标签行内：elm.style.width、elm.style.height、elm.style.backgroundColor
- 通过样式属性设置宽高、位置的属性类型是字符串，需要加上px



**类名操作**

- 修改标签的className属性相当于直接修改标签的类名



**创建元素的三种方式**

> document.write()

` document.write('新设置的内容<p>标签也可以生成</p>'); `



> innerHTML

`elm.innerHTML = '新内容<p>新标签</p>'; `



> document.createElement()

```js
var div = document.createElement('div');
document.body.appendChild(div); 
```



**性能问题**

- innerHTML方法由于会对字符串进行解析，需要避免在循环内多次使用。
- 可以借助字符串或数组的方式进行替换，再设置给innerHTML
- 优化后与document.createElement性能相近



**节点属性**

- nodeType  节点的类型
  - 元素节点
  - 属性节点
  - 文本节点 
- nodeName  节点的名称(标签名称)
- nodeValue  节点值（元素节点的nodeValue始终是null）



**节点层级**

```
节点操作，方法
	appendChild()
	insertBefore()
	removeChild()
	replaceChild()
节点层次，属性
	parentNode
	childNodes
	children
	nextSibling/previousSibling
	firstChild/lastChild
```



注意

- childNodes和children的区别，childNodes获取的是子节点，children获取的是子元素
- nextSibling和previousSibling获取的是节点，获取元素对应的属性是nextElementSibling和previousElementSibling

- nextElementSibling和previousElementSibling有兼容性问题，IE9以后才支持



#### 事件详解

**注册/移除事件的三种方式** 

```js
//方式一
var box = document.getElementById('box');
box.onclick = function () {
  console.log('点击后执行');
};
box.onclick = null;

//方式二
box.addEventListener('click', eventCode, false);
box.removeEventListener('click', eventCode, false);

//方式三
box.attachEvent('onclick', eventCode);
box.detachEvent('onclick', eventCode);

function eventCode() {
  console.log('点击后执行');
}
```



**兼容代码**

```js
function addEventListener(element, type, fn) {
  if (element.addEventListener) {
    element.addEventListener(type, fn, false);
  } else if (element.attachEvent){
    element.attachEvent('on' + type,fn);
  } else {
    element['on' + type] = fn;
  }
}

function removeEventListener(element, type, fn) {
  if (element.removeEventListener) {
    element.removeEventListener(type, fn, false);
  } else if (element.detachEvent) {
    element.detachEvent('on' + type, fn);
  } else {
    element['on'+type] = null;
  }
}
```



**事件的三个阶段**

1. 捕获阶段
2. 当前目标阶段
3. 冒泡阶段

​    事件对象.eventPhase属性可以查看事件触发时所处的阶段



**事件对象的属性和方法**

- event.type 获取事件类型
- clientX/clientY     所有浏览器都支持，窗口位置
- pageX/pageY       IE8以前不支持，页面位置
- event.target || event.srcElement 用于获取触发事件的元素
- event.preventDefault() 取消默认行为



**阻止事件传播的方式**

- 标准方式 event.stopPropagation();

- IE低版本 event.cancelBubble = true; 标准中已废弃



**常用的鼠标和键盘事件**

- onmouseup 鼠标按键放开时触发
- onmousedown 鼠标按键按下触发
- onmousemove 鼠标移动触发
- onkeyup 键盘按键抬起触发
- onkeydown 键盘按键按下触发



#### BOM

**BOM的概念**

- BOM(Browser Object Model) 是指浏览器对象模型，浏览器对象模型提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。BOM由多个对象组成，其中代表浏览器窗口的Window对象是BOM的顶层对象，其他对象都是该对象的子对象。

- 我们在浏览器中的一些操作都可以使用BOM的方式进行编程处理，比如：刷新浏览器、后退、前进、在浏览器中输入URL等



**BOM的顶级对象window**

- window是浏览器的顶级对象，当调用window下的属性和方法时，可以省略window
- 注意：window下一个特殊的属性 window.name



**对框**

- alert()

- prompt()
- confirm()



**页面加载事件**

- onload

```JS
window.onload = function () {
  // 当页面加载完成执行
  // 当页面完全加载所有内容（包括图像、脚本文件、CSS 文件等）执行
}
```

- onunload

```JS
window.onunload = function () {
  // 当用户退出页面时执行
}
```



**定时器**

- setTimeout()和clearTimeout()  在指定的毫秒数到达之后执行指定的函数，只执行一次

```JS
// 创建一个定时器，1000毫秒后执行，返回定时器的标示
var timerId = setTimeout(function () {
  console.log('Hello World');
}, 1000);

// 取消定时器的执行
clearTimeout(timerId);
```



- setInterval()和clearInterval()   定时调用的函数，可按照给定的时间(单位毫秒)周期调用函数

```JS
// 创建一个定时器，每隔1秒调用一次
var timerId = setInterval(function () {
  var date = new Date();
  console.log(date.toLocaleTimeString());
}, 1000);

// 取消定时器的执行
clearInterval(timerId);
```



**location对象**

- location对象是window对象下的一个属性，使用的时候可以省略window对象
- location可以获取或者设置浏览器地址栏的URL



**location有哪些成员？**

- 使用chrome的控制台查看
- 查MDN
- [MDN](https://developer.mozilla.org/zh-CN/)
- 成员
  - assign()/reload()/replace()
  - hash/host/hostname/search/href…



**URL——**统一资源定位符 (Uniform Resource Locator, URL)

- URL的组成

```
scheme://host:port/path?query#fragment
http://www.itheima.com:80/a/b/index.html?name=zs&age=18#bottom

scheme:通信协议
	常用的http,ftp,maito等
host:主机
	服务器(计算机)域名系统 (DNS) 主机名或 IP 地址。
port:端口号
	整数，可选，省略时使用方案的默认端口，如http的默认端口为80。
path:路径
	由零或多个'/'符号隔开的字符串，一般用来表示主机上的一个目录或文件地址。
query:查询
	可选，用于给动态网页传递参数，可有多个参数，用'&'符号隔开，每个参数的名和值用'='符号隔开。例如：name=zs
fragment:信息片断
	字符串，锚点.
```



**解析URL中的query，并返回对象的形式**

```JS
function getQuery(queryStr) {
  var query = {};
  if (queryStr.indexOf('?') > -1) {
    var index = queryStr.indexOf('?');
    queryStr = queryStr.substr(index + 1);
    var array = queryStr.split('&');
    for (var i = 0; i < array.length; i++) {
      var tmpArr = array[i].split('=');
      if (tmpArr.length === 2) {
        query[tmpArr[0]] = tmpArr[1];
      }
    } 
  }
  return query;
}
console.log(getQuery(location.search));
console.log(getQuery(location.href));
```



**history对象**

- back()
- forward()
- go()



**navigator对象**

- userAgent



####  特效

**偏移量**

- offsetParent用于获取定位的父级元素
- offsetParent和parentNode的区别

![img](C:/Users/eit/AppData/Local/YNote/data/qqF42A4000E9132802C0C24D831C0F51EC/3edf8e4ba3a04930b319c8157c4757e4/1498743216279.png)



**客户区大小**

- clientLeft、clientTop、clientWidth、clientHeight

![img](C:/Users/eit/AppData/Local/YNote/data/qqF42A4000E9132802C0C24D831C0F51EC/ae923f24fc4d43488bca94fc313722e1/1504075813134.png)

**滚动偏移**

- scrollLeft、scrollTop、scrollWidth、scrollHeight

![img](C:/Users/eit/AppData/Local/YNote/data/qqF42A4000E9132802C0C24D831C0F51EC/e9a9e8e155054d00845057ee611fbf45/1498743288621.png)



**案例** 

- 拖拽案例
- 弹出登录窗口
- 放大镜案例
- 模拟滚动条
- 匀速动画函数
- 变速动画函数
- 无缝轮播图
- 回到顶部  



**元素的类型**

![img](C:/Users/eit/AppData/Local/YNote/data/qqF42A4000E9132802C0C24D831C0F51EC/d5d40c95ef204dc6ad3da12888a41696/1497169919418.png)