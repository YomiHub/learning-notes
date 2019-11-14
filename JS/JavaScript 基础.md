### JavaScript 基础

- JavaScript组成：ECMAScript（javascript语法和基本对象）、DOM（文档对象）、BOM（浏览器对象）

- 注意声明变量的时候不要使用[关键字](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Lexical_grammar#%E5%85%B3%E9%94%AE%E5%AD%97)和[保留关键字](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Lexical_grammar#%E6%9C%AA%E6%9D%A5%E4%BF%9D%E7%95%99%E5%85%B3%E9%94%AE%E5%AD%97)：允许字母、数字、下划线、美元符任意组合，但是不允许数字开头（函数名、函数参数、属性名这些标识符同样适用该规则）



**JavaScript是什么**

- JavaScript是一种运行在客户端的脚本语言，浏览器是最常见的 JavaScript 宿主环境，但是在很多非浏览器环境中也使用 JavaScript ，例如 node.js

- JavaScript的解释器被称为JavaScript引擎，为浏览器的一部分，广泛用于客户端的脚本语言，最早是在HTML（标准通用标记语言下的一个应用）网页上使用，用来给HTML网页增加动态功能。



**JavaScript和HTML、CSS的区别**

HTML：提供网页的结构，提供网页中的内容

CSS: 用来美化网页

JavaScript: 可以用来控制网页内容，给网页增加动态的效果



**ECMAScript - JavaScript的核心**

> ECMA 欧洲计算机制造联合会

是JavaScript的核心，描述了语言的基本语法和数据类型，ECMAScript是一套标准，定义了一种语言的标准与具体实现无关（ECMAScript 2015 = ES6）

- 语法规范

  变量、数据类型、类型转换、操作符

  流程控制语句：判断、循环语句

  数组、函数、作用域、预解析

  对象、属性、方法、简单类型和复杂类型的区别

  内置对象：Math、Date、Array，基本包装类型String、Number、Boolean

  

**BOM - 浏览器对象模型**

- 一套操作浏览器功能的API

- 通过BOM可以操作浏览器窗口，比如：弹出框、控制浏览器跳转、获取分辨率等



**DOM - 文档对象模型**

- 一套操作页面元素的API
  - OM可以把HTML看做是文档树，通过DOM提供的API可以对树上的节点进行操作
    - 获取页面元素，注册事件
    - 属性操作，样式操作
    - 节点属性，节点层级
    - 动态创建元素

事件：注册事件的方式、事件的三个阶段、事件对象



**JavaScript的书写位置**

```html
/*写在行内*/
<input type="button" value="按钮" onclick="alert('Hello World')" />

/*写在script标签中*/
<head>
   <meta charset="UTF-8">
   <title></title>
   <script>window.onload = function(){}  //等待窗口加载完成</script>
</head>
<body>
//推荐将script放在body标签之后
  <script>
        alert('Hello World!');
  </script>
</body>

/*写在外部js文件中，在页面引入*/
<script type="text/javascript" src="main.js"></script>

/注意：引用外部文件的script标签中不可以书写javascript/
```



**计算机组成**

- 软件

  应用软件：浏览器(Chrome/IE/Firefox)、QQ、Sublime、Word

  系统软件：Windows、Linux、mac OSX

  

- 硬件

  三大件：CPU、内存、硬盘 \-- 主板

  输入设备：鼠标、键盘、手写板、摄像头等

  输出设备：显示器、打印机、投影仪等



**变量**

> 变量是计算机内存中存储数据的标识符，根据变量名称可以获取到内存中存储的数据；使用变量可以方便的获取或者修改内存中的数据

**如何使用变量**

```js
/* var声明变量 */
var age;

/* 变量的赋值 */
var age;
age = 18;

/* 同时声明多个变量 */
var age, name, sex;

/* 同时声明多个变量并赋值 */
var age = 10, name = 'zs';
```



**变量的命名规则和规范**

- 规则 - 必须遵守的，不遵守会报错
  - 由字母、数字、下划线、\$符号组成，不能以数字开头
  - 不能是关键字和保留字，例如：for、while。
  - 区分大小写



- 规范 - 建议遵守的，不遵守不会报错
  - 变量名必须有意义
  - 遵守驼峰命名法。首字母小写，后面单词的首字母需要大写。例如：userName、userPassword



#### 数据类型

**简单数据类型**

- Number、String、Boolean、Undefined、Null



**Number类型**

- 数值字面量：数值的固定值的表示法 ------ 字面量是指由字母,数字等构成的字符串或者数值



**进制**

```
十进制
	var num = 9;
	进行算数计算时，八进制和十六进制表示的数值最终都将被转换成十进制数值。
 
十六进制
	var num = 0xA;
	数字序列范围：0~9以及A~F
 
八进制
    var num1 = 07;   // 对应十进制的7
    var num2 = 019;  // 对应十进制的19
   数字序列范围：0~7
   若字面值中的数值超出范围，那么前导零将被忽略，后面的数值将被当作十进制数值解析
```



**浮点数（不要判断两个浮点数是否相等）**

```
浮点数
	var n = 5e-324;   // 科学计数法  5乘以10的-324次方  
 
浮点数值的最高精度是 17 位小数，但在进行算术计算时其精确度远远不如整数
   var result = 0.1 + 0.2;    // 结果不是 0.3，而是：0.30000000000000004
   console.log(0.07 * 100);
```



**数值范围**

```
最小值：Number.MIN_VALUE，这个值为： 5e-324
最大值：Number.MAX_VALUE，这个值为： 1.7976931348623157e+308
无穷大：Infinity
无穷小：-Infinity
```



**数值判断**

- NaN：not a number ------ NaN 与任何值都不相等，包括他本身

- isNaN: is not a number



**String类型** ------ \'abc\'、\"abc\"

- 字符串字面量

- 转义字符

------------------------- --------------------------------------------------------
  字面量                   			 含义
  \\n、\\t、\\b、\\r、\\f   	换行、制表、空格、回车、走纸换页
  \\\\ 、\\\'、\\\"         			 斜杠、单引号、双引号
  \\xnn                    		  以16进制代码nn表示的一个字符（其中n为0\~F）。
  \\unnnn                 	    以十六进制代码nnnn表示的一个Unicode字符（其中n为0\~F）

------------------------- --------------------------------------------------------

- 字符串长度
  - length属性用来获取字符串的长度

` console.log(str.length);`



**Boolean类型**

- Boolean字面量： true和false，区分大小写

- 计算机内部存储：true为1，false为0



**Undefined 和 Null**

- undefined表示一个声明了没有赋值的变量，变量只声明的时候值默认是undefined

- null表示一个空，变量的值如果想为null，必须手动设置



**symbol**

- symbol类型时es6新增的，表示唯一值/匿名的值

- var e = Symbol(); console.log(e); //打印的是 Symbol()



**复杂数据类型**

- Object(数组a = \[1,2,3\]、json a = {name:\'yanmei\'} ···)

- 常见的对象类型：Array、Object、Element、Elements（多个元素）、Function

```js
var fn = function(){
    
}

//打印出来的是function,但仍然归属与object只是typeof将它归属为function
console.log(typeof fn);  
```



**获取变量的类型** ------ typeof

```js
var age = 18;
console.log(typeof age);  // 'number'
```



**字面量------在源代码中一个固定值的表示法**

- 数值字面量：8, 9, 10

- 字符串字面量：\'黑马程序员\', \"大前端\"

- 布尔字面量：true，false



**数据类型转换**

——tyoeof的结果肯定是字符串： typeof 1 == \'bumber\'

> 转换成字符串类型

- toString()

```js
var num = 5;
console.log(num.toString());
```

- String()

```html
String()函数存在的意义：有些值没有toString()，这个时候可以使用String()。
比如：undefined和null
```

- 拼接字符串方式
  - num + \"\"，当 + 两边一个操作符是字符串类型，一个操作符是其它类型的时候，会先把其它类型转换成字符串再进行字符串拼接，返回字符串



> 转换成数值类型（NaN表示转换失败------首字符是非数字的）

- Number()------不允许非数值的字符：建议使用

```js
Number()可以把任意值转换成数值，如果要转换的字符串中有一个不是数值的字符，返回NaN

var arr = [1,2,3];
console.log(Number(arr));//打印出来是NaN

var arr1 = [10];
console.log(Number(arr1));  //打印出来的是10

//Js内部方法toString();将数组转成字符串；空字符转为0
```



- parseInt()转整数

```js
var num1 = parseInt("12.3abc"); 
//返回12，若第一个字符是数字会解析直到遇到非数字结束

var num2 = parseInt("abc123"); 
//返回NaN，如果第一个字符不是数字或者符号就返回NaN，当字符串和数字相加时刻可使用
```



- parseFloat()：只要字符串内含有非数字字符串就转成NaN

```
parseFloat()  //把字符串转换成浮点数
parseFloat()和parseInt非常相似，不同之处在与
	parseFloat会解析第一个. 遇到第二个.或者非数字结束
	如果解析的内容里只有整数，解析成整数
```



- +，-0等运算

```js
var str = '500';
console.log(+str);		// 取正
console.log(-str);		// 取负
console.log(str - 0);

console.log(NaN+2);   //NaN，其中一个操作数是NaN，那么结果就是NaN
console.log(null+1);  //1   调用了number(null)的转换，将null转换为0
```



**转换成布尔类型**

- Boolean()：0 "(空字符串)" null undefined NaN 会转换成false 其它都会转换成true



**隐式类型转换**

- 例如：当进行减法运算符的时候，当有一侧的操作数不是数字，JS就会自动将操作数转为数字再计算

- 隐式转换规则：[[ECMA官网]](http://www.ecma-international.org/ecma-262/6.0/#sec-addition-operator-plus)
  - 例如：

    - null == undefined; \'5\'==5 (会进行类型转换，但是换成===则不成立）

    - alert(true/2) 结果为0.5

    - alert(10/0) 结果为infinity，因为0不可以作为除数

    - alert(0/0) 结果为NaN

    - alert(10%0) 、 alert(0%0) 结果都为NaN



**NaN算不算数字**

- typeof NaN是number类型，但是不是一个具体的数字NaN!=NaN

- NaN不等于任何值，用isNaN(参数)判断是否是NaN

```js
//判断是否是NaN时
if(typeof arr[i] == 'number' && isNaN(arr[i])){
    arrNew.push(arr[i])
}
```



**infinity 无穷大**（计算机可计算的范围）

- 有两个：-infinity、infinity



#### 操作符

运算符 operator

**算术运算符**

+ \+ - * / % （- 减号也是加性操作符之一，a+(-b) = a-b）

- 如果左右都是数值，运算符 - 执行正常减法计算；出现两个 运算符 - 时，会变为 +

- 如果任何一个操作数是NaN，那么结果就是NaN

- 左右任何一侧不是Number类型的情况下，会调用Number进行转换，将其转换为Number类型再计算



**一元运算符**

- 一元运算符：只有一个操作数的运算符

- 二元运算符：5 + 6 两个操作数的运算符



**布尔运算符**

- &&与、\|\|或、！非



**关系运算符(比较运算符)**

```js
<  >  >=  <=  == != === !==

/* ==与===的区别：==只进行值得比较，===类型和值同时相等，则相等 */

var result = '55' == 55;  	// true
var result = '55' === 55; 	// false 值相等，类型不相等
var result = 55 === 55; 	// true
```



**赋值运算符**

- = += -= \*= /= %=



**运算符的优先级**

优先级从高到底

```js
1. ()  优先级最高
2. 一元运算符  ++   --   !
3. 算数运算符  先*  /  %   后 +   -
4. 关系运算符  >   >=   <   <=
5. 相等运算符   ==   !=    ===    !==
6. 逻辑运算符 先&&   后||
7. 赋值运算符
```



#### 表达式和语句

**表达式**

- 一个表达式可以产生一个值，有可能是运算、函数调用、有可能是字面量。表达式可以放在任何需要值的地方



**语句**

- 语句可以理解为一个行为，循环语句和判断语句就是典型的语句。，一般情况下,一个程序有很多个语句组成



**流程控制**

- 顺序结构：从上到下执行的代码就是顺序结构，程序默认就是由上到下顺序执行的

- 分支结构：根据不同的情况，执行对应代码

- 循环结构：重复做一件事情



#### 分支结构

**if语句**

```js
if (/* 条件1 */){
  // 成立执行语句
} else if (/* 条件2 */){
  // 成立执行语句
} else if (/* 条件3 */){
  // 成立执行语句
} else {
  // 最后默认执行语句
}

// 闰年：能被4整除，但不能被100整除的年份 或者 能被400整除的年份
```



**三元运算符**

```js
表达式1 ? 表达式2 : 表达式3
/* 是对if……else语句的一种简化写法 ,表达式1成立则返回表达式2，否则就返回表达式3*/
```



**switch语句**

```
switch (expression) {
  case 常量1:
    语句;
    break;   /*添加了break才会中断，否则会一直顺序执行*/
  …
  case 常量n:
    语句;
    break;
  default: /* 不存在符合的条件时才会执行 */
    语句;
    break;
}

/* 案例 */
switch((int)(score/10))
{    
    case 10:    
    case 9:printf("A(最好)\n");break;    
    case 8:printf("B(优秀)\n");break;    
    case 7:printf("C(良好)\n");break;    
    case 6:printf("D(及格)\n");break;    
    case 5:
    case 4:
    case 3:
    case 2:
    case 1:
    case 0:printf("E(不及格)\n");break;
    default:printf("Error!\n");
}
```

- break可以省略，如果省略，代码会继续执行下一个case

- switch 语句在比较值时使用的是全等操作符, 因此不会发生类型转换（例如，字符串\'10\' 不等于数值 10）



**布尔类型的隐式转换**

流程控制语句会把后面的值隐式转换成布尔类型

```js
转换为true   非空字符串  非0数字  true 任何对象
转换成false  空字符串  0  false  null  undefined

/* 案例 */
var message;
// 会自动把message转换成false
if (message) {     
  // todo...
}
```



**循环结构**

------ 在javascript中，循环语句有三种，while、do..while、for循环。

**while语句**

- 当循环条件为true时，执行循环体；当条件为false时，结束循环体

```js
// 计算1-100之间所有数的和
// 初始化变量
var i = 1;
var sum = 0;
// 判断条件
while (i <= 100) {
  // 循环体
  sum += i;
  // 自增
  i++;
}
console.log(sum);
```



**do\...while语句**

- do..while循环和while循环非常像，二者经常可以相互替代，但是do..while的特点是不管条件成不成立，都会执行一次。

```js
// 初始化变量
var i = 1;
var sum = 0;
do {
  sum += i;//循环体
  i++;//自增
} while (i <= 100);//循环条件

var test = prompt("请输入数据:"，"预定输入信息this is a JavaScript")
```



**for语句**

------ while和do\...while一般用来解决无法确认次数的循环，for循环一般在循环次数确定时比较方便

- 执行顺序：1243 \-\-\-- 243 \-\-\-\--243(直到循环条件变成false)

  初始化表达式

  判断表达式

  自增表达式

  循环体

- 当不写循环条件或者变量自增时会出现死循环

```js
/* 打印直角三角形 */
var start = '';
for (var i = 0; i < 10; i++) {
  for (var j = i; j < 10; j++) {
    start += '* ';
  }
  start += '\n';
}
console.log(start);

/* 打印9*9乘法表 */
var str = '';
for (var i = 1; i <= 9; i++) {
  for (var j = i; j <=9; j++) {
    str += i + ' * ' + j + ' = ' + i * j + '\t';
  }
  str += '\n';
}
console.log(str);

    //for循环 在页面加载时执行，也就是说当i等于2的时候才会结束循环
for(var i = 0; i<2; i++){
  console.log(i);   //打印0  1
  
  //点击事件是在页面加载完成之后才进行点击
  elm.onclick = function(){
      console.log(i);  //打印 2
      this.style.background = 'red';    //用this来控制
  }
}
```



```js
//可以把i的定义和自增放在for的括号之外
var i=0;
for(;i<3;){
    console.log();
    i++;
}
```



> 思考题：有个人想知道，一年之内一对兔子能繁殖多少对？于是就筑了一道围墙把一对兔子关在里面。已知一对兔子每个月可以生一对小兔子，而一对兔子从出生后第3个月起每月生一对小兔子。假如一年内没有发生死亡现象，那么，一对兔子一年内（12个月）能繁殖成多少对？（兔子的规律为数列，1，1，2，3，5，8，13，21）



**continue和break**

- break:立即跳出整个循环，即循环结束，开始执行循环后面的内容（直接跳到大括号）

- continue:立即跳出当前循环，继续下一次循环（跳到i++的地方）



**调试**

- 过去调试JavaScript的方式
  - alert()
  - console.log()

- 断点调试
  - 断点调试是指自己在程序的某一行设置一个断点，调试时，程序运行到这一行就会停住，然后你可以一步一步往下调试，调试过程中可以看各个变量当前的值，出错的话，调试到出错的代码行即显示错误，停下。
  - 调试步骤
    - 浏览器中按F12\--\>sources\--\>找到需要调试的文件\--\>在程序的某一行设置断点



- 调试中的相关操作
  - Watch: 监视，通过watch可以监视变量的值的变化，非常的常用。
  - F10: 程序单步执行，让程序一行一行的执行，这个时候，观察watch中变量的值的变化
  - F8：跳到下一个断点处，如果后面没有断点了，则程序执行结束。

------ tips: 监视变量，不要监视表达式，因为监视了表达式，那么这个表达式也会执行。



#### 数组

------ 所谓数组，就是将多个元素（通常是同一类型）按一定顺序排列放到一个集合中，那么这个集合我们就称之为数组。

**数组的定义**

- 数组是一个有序的列表，可以在数组中存放任意的数据，并且数组的长度可以动态的调整。

```js
// 创建一个空数组
var arr1 = []; 
// 创建一个包含3个数值的数组，多个数组项以逗号隔开
var arr2 = [1, 3, 4]; 
// 创建一个包含2个字符串的数组
var arr3 = ['a', 'c']; 

// 可以通过数组的length属性获取数组的长度
console.log(arr3.length);
// 可以设置length属性改变数组中元素的个数
arr3.length = 0;
```



**获取数组元素**

- 格式：数组名\[下标\] 下标又称索引

- 功能：获取数组对应下标的那个值，如果下标不存在，则返回undefined

```js
var arr = ['red', 'green', 'blue'];
arr[0];	// red
arr[2]; // blue
arr[3]; // 这个数组的最大下标为2,因此返回undefined
```



**遍历数组**

遍历：遍及所有，对数组的每一个元素都访问一次就叫遍历

```js
for(var i = 0; i < arr.length; i++) {
	// 数组遍历的固定结构
}
```



**数组中新增元素**

- 格式：数组名\[下标/索引\] = 值;

- 如果下标有对应的值，会把原来的值覆盖，如果下标不存在，会给数组新增一个元素。

```js
var arr = ["red", "green", "blue"];
// 把red替换成了yellow
arr[0] = "yellow";
// 给数组新增加了一个pink的值
arr[3] = "pink";

```



**函数**

------ 把一段相对独立的具有特定功能的代码块封装起来，形成一个独立实体，就是函数，起个名字（函数名），在后续开发中可以反复调用（封装代码，重复使用）

- 函数的定义
  - 函数声明

```js
function 函数名() {
  // 函数体
}
```



- 函数表达式

```js
var fn = function () {
  // 函数体
}
```



**函数的特点**

- 函数声明的时候，函数体并不会执行，只有当函数被调用的时候才会执行。

- 函数一般都用来干一件事情，函数名称一般使用动词



**函数的调用**

- 调用函数的语法：函数名();
- 特点：调用需要()进行调用。可以调用多次(重复使用)



**函数的参数**

函数内部是一个封闭的环境，可以通过参数的方式，把外部的值传递给函数内部

```js
// 带参数的函数声明
function 函数名(形参1, 形参2, 形参3...) {
  // 函数体
}

// 带参数的函数调用
函数名(实参1, 实参2, 实参3); 
形参1 = 实参1
形参2 = 实参2

/* 案例 */
判断一个数是否是素数(又叫质数，只能被1和自身整数的数)
```



- 形式参数：在声明一个函数的时候，为了函数的功能更加灵活，有些值是固定不了的，对于这些固定不了的值。我们可以给函数设置参数。这个参数没有具体的值，仅仅起到一个占位置的作用，我们通常称之为形式参数，也叫形参。

- 实际参数：如果函数在声明时，设置了形参，那么在函数调用的时候就需要传入对应的参数，我们把传入的参数叫做实际参数，也叫实参。



**函数的返回值**

- 当函数执行完时，函数通过return返回一个返回值

```js
//声明一个带返回值的函数
function 函数名(形参1, 形参2, 形参3...) {
  //函数体
  return 返回值;
}

//可以通过变量来接收这个返回值
var 变量 = 函数名(实参1, 实参2, 实参3...);

/* 求阶乘 */
求1!+2!+3!+....+n!
```



- 如果函数没有显示的使用 return语句 ，那么函数有默认的返回值：undefined

- 如果函数使用 return语句，但是return后面没有任何值，那么函数的返回值也是：undefined

- 函数使用return语句后，这个函数会在执行完 return 语句之后停止并立即退出，也就是说return后面的所有其他代码都不会再执行。



**arguments的使用**

- JavaScript中，arguments对象是比较特别的一个对象，实际上是当前函数的一个内置属性。也就是说所有函数都内置了一个arguments对象，arguments对象中存储了传递的所有的实参。arguments是一个伪数组，因此及可以进行遍历

```js
求斐波那契数列Fibonacci中的第n个数是多少？      1 1 2 3 5 8 13 21...
翻转数组，返回一个新数组
对数组排序，从小到大
输入一个年份，判断是否是闰年[闰年：能被4整数并且不能被100整数，或者能被400整数]
输入某年某月某日，判断这一天是这一年的第几天？
```



#### 函数其它

**匿名函数：没有名字的函数**

- 将匿名函数赋值给一个变量，这样就可以通过变量进行调用

- 匿名函数自调用



**自调用函数**

- 匿名函数不能通过直接调用来执行，因此可以通过匿名函数的自调用的方式来执行

```js
(function () {
  alert(123);
})();
```



**函数是一种数据类型**

```js
function fn() {}
console.log(typeof fn);
```



- 函数作为参数
  - 因为函数也是一种类型，可以把函数作为一个函数的参数，在另一个函数中调用

- 函数做为返回值
  - 因为函数是一种类型，所以可以把函数可以作为返回值从函数内部返回。

```js
function fn(b) {
  var a = 10;
  return function () {
    alert(a+b);
  }
}
fn(15)();
```



**代码规范**

- 命名规范

  - 变量、函数 的命名 必须要有意义
  - 变量 的名称一般用名词
  - 函数 的名称一般用动词

  

- 变量规范

  - 操作符的前后要有空格
  - var name = \'zs\'; 5 + 6

  

- 注释规范

`// 这里是注释`



- 空格规范

```js
if (true) {
      
}
for (var i = 0; i <= 100; i++) {
  
}
```



- 换行规范

```js
var arr = [1, 2, 3, 4];
if (a > b) {
  
}
for (var i = 0; i < 10; i++) {
  
}
function fn() {
  
}
```



#### 作用域

**全局变量和局部变量**

- 全局变量
  - 在任何地方都可以访问到的变量就是全局变量，对应全局作用域

- 局部变量
  - 只在固定代码片段内可访问，常见的例如函数内部，对应局部作用域(函数作用域)
  - 不使用var声明的变量是全局变量，不推荐使用。

局部变量退出作用域之后会销毁，全局变量：关闭网页或浏览器才会销毁



**块级作用域**

- 任何一对花括号（｛和｝）中的语句集都属于一个块，在这之中定义的所有变量在代码块外都是不可见的，我们称之为块级作用域。

- 在es5之前没有块级作用域的的概念，只有函数作用域，现阶段可以认为JavaScript没有块级作用域



**作用域链**

- 只有函数可以制造作用域结构， 那么只要是代码，就至少有一个作用域, 即全局作用域。凡是代码中有函数，那么这个函数就构成另一个作用域。如果函数中还有函数，那么在这个作用域中就又可以诞生一个作用域。

- 将这样的所有的作用域列出来，可以有一个结构: 函数内指向函数外的链式结构。就称作作用域链



**预解析**

- JavaScript代码的执行是由浏览器中的JavaScript解析器来执行的。JavaScript解析器执行JavaScript代码的时候，分为两个过程：预解析过程和代码执行过程

- 预解析过程：

  1\. 把变量的声明提升到当前作用域的最前面，只会提升声明，不会提升赋值。

  2\. 把函数的声明提升到当前作用域的最前面，只会提升声明，不会提升调用。

  3\. 先提升var，再提升function。

```js
// 案例1
var a = 25;
function abc() {
  alert(a); 
  var a = 10;
}
abc();


// 案例2
console.log(a);
function a() {
  console.log('aaaaa');
}
var a = 1;
console.log(a);

```



**变量提升**

- 变量提升
  - 定义变量的时候，变量的声明会被提升到作用域的最上面，变量的赋值不会提升。

- 函数提升
  - JavaScript解析器首先会把当前作用域的函数声明提前到整个作用域的最前面

```js
// 1、-----------------------------------
var num = 10;
fun();
function fun() {
  console.log(num);
  var num = 20;
}
//2、-----------------------------------
var a = 18;
f1();
function f1() {
  var b = 9;
  console.log(a);
  console.log(b);
  var a = '123';
}
// 3、-----------------------------------
f1();
console.log(c);
console.log(b);
console.log(a);
function f1() {
  var a = b = c = 9;
  console.log(a);
  console.log(b);
  console.log(c);
}
```



#### 对象

**为什么要有对象**

```js
function printPerson(name, age, sex....) {
}
// 函数的参数如果特别多的话，可以使用对象简化
function printPerson(person) {
  console.log(person.name);
  ……
}
```



**JavaScript中的对象**

- JavaScript的对象是无序属性的集合
  - 其属性可以包含基本值、对象或函数。对象就是一组没有顺序的值。我们可以把JavaScript中的对象想象成键值对，其中值可以是数据和函数。

- 对象的行为和特征
  - 特征\-\--属性
  - 行为\-\--方法



**对象字面量**

- 字面量：11 \'abc\' true \[\] {}等

```js
function printPerson(name, age, sex....) {
}
// 函数的参数如果特别多的话，可以使用对象简化
function printPerson(person) {
  console.log(person.name);
  ……
}
```



**对象创建方式**

- 对象字面量

```js
var o = {
  name: 'zs',
  age: 18,
  sex: true,
  sayHi: function () {
    console.log(this.name);
  }
};   
```



- new Object()创建对象

```js
var person = new Object();
person.name = 'lisi';
person.age = 35;
person.job = 'actor';
person.sayHi = function() {
  console.log('Hello,everyBody');
}
```



工厂函数创建对象

```js
function createPerson(name, age, job) {
  var person = new Object();
  person.name = name;
  person.age = age;
  person.job = job;
  person.sayHi = function(){
    console.log('Hello,everyBody');
  }
  return person;
}
var p1 = createPerson('张三', 22, 'actor');
```



- 自定义构造函数

```js
function Person(name, age, job){
  this.name = name;
  this.age = age;
  this.job = job;
  this.sayHi = function(){
  	console.log('Hello,everyBody');
  }
}
var p1 = new Person('张三', 22, 'actor');
```



**属性和方法**

- 如果一个变量属于一个对象所有，那么该变量就可以称之为该对象的一个属性，属性一般是名词，用来描述事物的特征

- 如果一个函数属于一个对象所有，那么该函数就可以称之为该对象的一个方法，方法是动词，描述事物的行为和功能



**new关键字**

- 构造函数 ，是一种特殊的函数。主要用来在创建对象时初始化对象， 即为对象成员变量赋初始值，总与new运算符一起使用在创建对象的语句中
  - 构造函数用于创建一类对象，首字母要大写。
  - 构造函数要和new一起使用才有意义

- new在执行时会做四件事情
  - new会在内存中创建一个新的空对象
  - new 会让this指向这个新的对象
  - 执行构造函数 目的：给这个新对象加属性和方法
  - new会返回这个新对象



**this详解**

函数内部的this几个特点

- 1\. 函数在定义的时候this是不确定的，只有在调用的时候才可以确定
- 2\. 一般函数直接执行，内部this指向全局window
- 3\. 函数作为一个对象的方法，被该对象所调用，那么this指向的是该对象
- 4\. 构造函数中的this其实是一个隐式对象，类似一个初始化的模型，所有方法和属性都挂载到了这个隐式对象身上，后续通过new关键字来调用，从而实现实例化



#### 对象的使用

**遍历对象的属性**

- 通过for..in语法可以遍历一个对象

```js
var obj = {};
for (var i = 0; i < 10; i++) {
  obj[i] = i * 2;
}
for(var key in obj) {
  console.log(key + "==" + obj[key]);
}
```



**删除对象的属性**

```js
function fun() { 
  this.name = 'mm';
}
var obj = new fun(); 
console.log(obj.name); // mm 
delete obj.name;
console.log(obj.name); // undefined
```



**简单类型和复杂类型的区别**

- 基本类型又叫做值类型，复杂类型又叫做引用类型
  - 值类型：简单数据类型，基本数据类型，在存储时，变量中存储的是值本身，因此叫做值类型
  - 引用类型：复杂数据类型，变量中存储的仅仅是地址（引用），因此叫做引用数据类型



- 堆和栈 ------ 堆栈空间分配区别：

　　1、栈（操作系统）：由操作系统自动分配释放 ，存放函数的参数值，局部变量的值等。

　　2、堆（操作系统）： 存储复杂类型(对象)，一般由程序员分配释放， 若程序员不释放，由垃圾回收机制回收。

------ 注意：JavaScript中没有堆和栈的概念，此处我们用堆和栈来讲解，目的方便理解和方便以后的学习

```js
// 下面代码输出的结果?
function Person(name,age,salary) {
  this.name = name;
  this.age = age;
  this.salary = salary;
}
function f1(person) {
  person.name = "ls";
  person = new Person("aa",18,10);
}

var p = new Person("zs",18,1000);
console.log(p.name);
f1(p);
console.log(p.name);
```

```js
/* 思考 */
//1. 
var num1 = 10;
var num2 = num1;
num1 = 20;
console.log(num1);
console.log(num2);

//2. 
var num = 50;
function f1(num) {
    num = 60;
    console.log(num);
}
f1(num);
console.log(num);
```



**内置对象**

------ JavaScript中的对象分为3种：内置对象、自定义对象、浏览器对象。JavaScript 提供多个内置对象：Math/Array/Date\....

------ 对象只是带有属性和方法的特殊数据类型



- MDN ------ https://developer.mozilla.org/zh-CN/
  - Mozilla 开发者网络（MDN）提供有关开放网络技术（Open Web）的信息，包括 HTML、CSS 和万维网及 HTML5 应用的 API
  - Math.random() 函数返回一个浮点, 伪随机数在范围\[0，1)，也就是说，从0（包括0）往上，但是不包括1（排除1）

- 方法？
  - 方法的功能
  - 参数的意义和**类型**
  - 返回值意义和**类型**
  - demo进行测试



- Math对象
  - Math对象不是构造函数，它具有数学常数和函数的属性和方法，都是以静态成员的方式提供跟数学相关的运算来找Math中的成员（求绝对值，取整）

  

  - 演示：Math.PI、Math.random()、Math.floor()/Math.ceil()、Math.round()、Math.abs() 、Math.max()

```js
Math.PI						// 圆周率
Math.random()				// 生成随机数
Math.floor()/Math.ceil()	 // 向下取整/向上取整
Math.round()				// 取整，四舍五入
Math.abs()					// 绝对值
Math.max()/Math.min()		 // 求最大和最小值

Math.sin()/Math.cos()		 // 正弦/余弦
Math.power()/Math.sqrt()	 // 求指数次幂/求平方根

/* 案例 */
- 求10-20之间的随机数
- 随机生成颜色RGB
- 模拟实现max()/min()
```



**Date对象**

- 创建 \`Date\` 实例用来处理日期和时间。Date 对象基于1970年1月1日（世界标准时间）起的毫秒数

```js
// 获取当前时间，UTC世界时间，距1970年1月1日（世界标准时间）起的毫秒数
var now = new Date();
console.log(now.valueOf());	// 获取距1970年1月1日（世界标准时间）起的毫秒数

Date构造函数的参数
1. 毫秒数 1498099000356		new Date(1498099000356)
2. 日期格式字符串  '2015-5-1'	 new Date('2015-5-1')
3. 年、月、日……		  new Date(2015, 4, 1)   // 月份从0开始
```



- 获取日期的毫秒形式

```js
var now = new Date();
// valueOf用于获取对象的原始值
console.log(date.valueOf())	

// HTML5中提供的方法，有兼容性问题
var now = Date.now();	

// 不支持HTML5的浏览器，可以用下面这种方式
var now = + new Date();			// 调用 Date对象的valueOf() 
```



- 日期格式化方法

```js
toString()		// 转换成字符串
valueOf()		// 获取毫秒值
// 下面格式化日期的方法，在不同浏览器可能表现不一致，一般不用
toDateString()
toTimeString()
toLocaleDateString()
toLocaleTimeString()
```



- 获取日期指定部分

```js
getTime()  	  // 返回毫秒数和valueOf()结果一样
getMilliseconds() 
getSeconds()  // 返回0-59
getMinutes()  // 返回0-59
getHours()    // 返回0-23
getDay()      // 返回星期几 0周日   6周6
getDate()     // 返回当前月的第几天
getMonth()    // 返回月份，***从0开始***
getFullYear() //返回4位的年份  如 2016
```



> 案例 ------ 写一个函数，格式化日期对象，返回yyyy-MM-dd HH:mm:ss的形式

```js
function formatDate(d) {
  //如果date不是日期对象，返回
  if (!date instanceof Date) {
    return;
  }
  var year = d.getFullYear(),
      month = d.getMonth() + 1, 
      date = d.getDate(), 
      hour = d.getHours(), 
      minute = d.getMinutes(), 
      second = d.getSeconds();
  month = month < 10 ? '0' + month : month;
  date = date < 10 ? '0' + date : date;
  hour = hour < 10 ? '0' + hour : hour;
  minute = minute < 10 ? '0' + minute:minute;
  second = second < 10 ? '0' + second:second;
  return year + '-' + month + '-' + date + ' ' + hour + ':' + minute + ':' + second;
}
```



> 案例 ------ 计算时间差，返回相差的天/时/分/秒

```js
function getInterval(start, end) {
  var day, hour, minute, second, interval;
  interval = end - start;
  interval /= 1000;
  day = Math.round(interval / 60 / 60 / 24);
  hour = Math.round(interval / 60 / 60 % 24);
  minute = Math.round(interval / 60 % 60);
  second = Math.round(interval % 60);
  return {
    day: day,
    hour: hour,
    minute: minute,
    second: second
  }
}
```



**Array对象**

- 创建数组对象的两种方式
  - 字面量方式
  - new Array()

```js
// 1. 使用构造函数创建数组对象
// 创建了一个空数组
var arr = new Array();
// 创建了一个数组，里面存放了3个字符串
var arr = new Array('zs', 'ls', 'ww');
// 创建了一个数组，里面存放了4个数字
var arr = new Array(1, 2, 3, 4);


// 2. 使用字面量创建数组对象
var arr = [1, 2, 3];
// 获取数组中元素的个数
console.log(arr.length);
```



- 检测一个对象是否是数组
  - instanceof
  - Array.isArray() HTML5中提供的方法，有兼容性问题。函数的参数，如果要求是一个数组的话，可以用这种方式来进行判断



- toString()/valueOf()
  - toString() 把数组转换成字符串，逗号分隔每一项
  - valueOf() 返回数组对象本身



- 数组常用方法

演示：push()、shift()、unshift()、reverse()、sort()、splice()、indexOf()

```js
// 1 栈操作(先进后出)
push()
pop() 		//取出数组中的最后一项，修改length属性

// 2 队列操作(先进先出)
push()
shift()		//取出数组中的第一个元素，修改length属性
unshift() 	//在数组最前面插入项，返回数组的长度

// 3 排序方法
reverse()	//翻转数组
sort(); 	//即使是数组sort也是根据字符，从小到大排序
// 带参数的sort是如何实现的？

// 4 操作方法
concat()  	
//把参数拼接到当前数组

slice() 	
//从当前数组中截取一个新的数组，不影响原来的数组，参数start从0开始,end从1开始

splice()	
//删除或替换当前数组的某些项目，参数start, deleteCount, options(要替换的项目)

// 5 位置方法
indexOf()、lastIndexOf()   //如果没找到返回-1

// 6 迭代方法 不会修改原数组(可选)  html5
every()、filter()、forEach()、map()、some()

// 7 方法将数组的所有元素连接到一个字符串中。
join()
```



- 清空数组

```js
// 方式1 推荐 
arr = [];
// 方式2 
arr.length = 0;
// 方式3
arr.splice(0, arr.length);
```



**案例**

- 将一个字符串数组输出为\|分割的形式，比如"刘备\|张飞\|关羽"。使用两种方式实现

```js
function myJoin(array, seperator) {
 seperator = seperator || ',';
 array = array || [];
 if (array.length == 0){
   return '';
}
 var str = array[0];
 for (var i = 1; i < array.length; i++) {
   str += seperator + array[i];
}
 return str;
}
var array = [6, 3, 5, 6, 7, 8, 0];
console.log(myJoin(array, '-'));

console.log(array.join('-'))
```



- 将一个字符串数组的元素的顺序进行反转。\[\"a\", \"b\", \"c\", \"d\"\] -\> \[ \"d\",\"c\",\"b\",\"a\"\]。使用两种种方式实现。提示：第i个和第length-i-1个进行交换

```js
function myReverse(arr) {
     if (!arr || arr.length == 0) {
       return [];
    }
     for (var i = 0; i < arr.length / 2; i++) {
       var tmp = arr[i];
       arr[i] = arr[this.length - i - 1];
       arr[arr.length - i - 1] = tmp;
    }
    return arr;
}

var array = ['a', 'b', 'c'];
console.log(myReverse(array));
console.log(array.reverse());
```



- 工资的数组\[1500, 1200, 2000, 2100, 1800\],把工资超过2000的删除

```js
// 方式1
var array = [1500,1200,2000,2100,1800];
var tmpArray = [];
for (var i = 0; i < array.length; i++) {
     if(array[i] < 2000) {
       tmpArray.push(array[i]);
    }
}
console.log(tmpArray);
// 方式2
var array = [1500, 1200, 2000, 2100, 1800];
array = array.filter(function (item, index) {
 if (item < 2000) {
   return true;
}
 return false;
});
console.log(array);
```



- [\"c\", \"a\", \"z\", \"a\", \"x\", \"a\"\]找到数组中每一个a出现的位置

```js
var array = ['c', 'a', 'z', 'a', 'x', 'a'];
do {
 var index = array.indexOf('a',index + 1);
 if (index != -1){
   console.log(index);
}
} while (index > 0);
```



- 编写一个方法去掉一个数组的重复元素

```js
var array = ['c', 'a', 'z', 'a', 'x', 'a'];
function clear() {
 var o = {};
 for (var i = 0; i < array.length; i++) {
   var item = array[i];
   if (o[item]) {
     o[item]++;
  }else{
     o[item] = 1;
  }
}
 var tmpArray = [];
 for(var key in o) {
   if (o[key] == 1) {
     tmpArray.push(key);
  }else{
     if(tmpArray.indexOf(key) == -1){
       tmpArray.push(key);
    }
  }
}
 return tmpArray;
}

console.log(clear(array));
```



**基本包装类型**

- 为了方便操作简单数据类型，JavaScript还提供了三个特殊的简单类型类型：String/Number/Boolean

```js
// s1是基本类型，基本类型是没有方法的
var s1 = 'zhangsan';
var s2 = s1.substring(5);
// 当调用s1.substring(5)的时候，先把s1包装成String类型的临时对象，
//再调用substring方法，最后销毁临时对象, 相当于：
var s1 = new String('zhangsan');
var s2 = s1.substring(5);
s1 = null;
// 创建基本包装类型的对象
var num = 18; //数值，基本类型
var num = Number('18'); //类型转换
var num = new Number(18); //基本包装类型，对象
// Number和Boolean基本包装类型基本不用，使用的话可能会引起歧义。例如：
var b1 = new Boolean(false);
var b2 = b1 && true; // 结果是什么
```



- String对象

字符串的不可变

```js
var str = 'abc';
str = 'hello';
// 当重新给str赋值的时候，常量'abc'不会被修改，依然在内存中
// 重新给字符串赋值，会重新在内存中开辟空间，这个特点就是字符串的不可变
// 由于字符串的不可变，在大量拼接字符串的时候会有效率问题
创建字符串对象
var str = new String('Hello World');

// 获取字符串中字符的个数
console.log(str.length);
```



字符串对象的常用方法

> 字符串所有的方法，都不会修改字符串本身(字符串是不可变的)，操作完成会返回一个新的字符串

```js
// 1 字符方法
charAt()   //获取指定位置处字符
charCodeAt() //获取指定位置处字符的ASCII码
str[0]   //HTML5，IE8+支持 和charAt()等
// 2 字符串操作方法
concat()   //拼接字符串，等效于+，+更常用
slice()   //从start位置开始，截取到end位置，end取不到
substring() //从start位置开始，截取到end位置， end取不到
substr()   //从start位置开始，截取length个字符
// 3 位置方法
indexOf()   //返回指定内容在元字符串中的位置
lastIndexOf() //从后往前找，只找第一个匹配的
// 4 去除空白  
trim() //只能去除字符串前后的空白
// 5 大小写转换方法
to(Locale)UpperCase() //转换大写
to(Locale)LowerCase() //转换小写
// 6 其它
search()
replace()
split()
```



- 案例

截取字符串\"我爱中华人民共和国\"，中的\"中华\"

```js
var s = "我爱中华人民共和国";
s = s.substr(2,2);
console.log(s);
```



\"abcoefoxyozzopp\"查找字符串中所有o出现的位置

```js
var s = 'abcoefoxyozzopp';
var array = [];
do {
 var index = s.indexOf('o', index + 1);  // (searchValue,fromIndex开始位置)
 if (index != -1) {
   array.push(index);
 }
} while (index > -1);
console.log(array);
```



把字符串中所有的o替换成!

```js
var s = 'abcoefoxyozzopp';
var index = -1;
do {
 index = s.indexOf('o', index + 1);
 if (index !== -1) {
   // 替换
   s = s.replace('o', '!');
}
} while(index !== -1);
console.log(s);
```



把字符串中的所有空白去掉\' abc xyz a 123 \'

```js
var s = '   abc       xyz a   123   ';  
var arr = s.split(' ');
console.log(arr.join(''));
```



判断一个字符串中出现次数最多的字符，统计这个次数

```js
var s = 'abcoefoxyozzopp';
var o = {};

for (var i = 0; i < s.length; i++) {
 var item = s.charAt(i);
 if (o[item]) {
   o[item] ++;
}else{
   o[item] = 1;
}
}

var max = 0;
var char ;
for(var key in o) {
 if (max < o[key]) {
   max = o[key];
   char = key;
}
}

console.log(max);
console.log(char);
```



获取url中?后面的内容，并转化成对象的形式。例如：[[http://www.itheima.com/login?name=zs&age=18&a=1&b=2]{.underline}](http://www.itheima.com/login?name=zs&age=18&a=1&b=2)

```js
var url = 'http://www.itheima.com/login?name=zs&age=18&a=1&b=2';
// 获取url后面的参数
function getParams(url) {
 // 获取? 后面第一个字符的索引
 var index = url.indexOf('?') + 1;
 // url中?后面的字符串 name=zs&age=18&a=1&b=2
 var params = url.substr(index);
 // 使用& 切割字符串 ，返回一个数组
 var arr = params.split('&');
 var o = {};
 // 数组中每一项的样子 key = value
 for (var i = 0; i < arr.length; i++) {
   var tmpArr = arr[i].split('=');
   var key = tmpArr[0];
   var value = tmpArr[1];

   o[key] = value;
}
 return o;
}

var obj = getParams(url);
console.log(obj);

console.log(obj.name);
console.log(obj.age);
```




