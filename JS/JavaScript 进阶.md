### JavaScript进阶

**浏览器如何工作**

- User Interface 用户界面，我们所看到的浏览器

- Browser engine 浏览器引擎，用来查询和操作渲染引擎

- Rendering engine 用来显示请求的内容，负责解析HTML、CSS，并把解析的内容显示出来

- Networking 网络，负责发送网络请求

- JavaScript Interpreter(解析者) JavaScript解析器，负责执行JavaScript的代码

- UI Backend UI后端，用来绘制类似组合框和弹出窗口

- Data Persistence(持久化) 数据持久化，数据存储 cookie、HTML5中的sessionStorage



**JavaScript 执行过程**

- 预解析
  - 全局预解析（所有变量和函数声明都会提前；同名的函数和变量函数的优先级高）
  - 函数内部预解析（所有的变量、函数和形参都会参与预解析）
    - 函数
    - 形参
    - 普通变量

- 执行
  - 先预解析全局作用域，然后执行全局作用域中的代码，在执行全局代码的过程中遇到函数调用就会先进行函数预解析，然后再执行函数内代码。



**JavaScript 面向对象编程**

**什么是对象**

- 对象是单个事物的抽象

- 对象是一个容器，封装了属性（property）和方法（method）

------ 在实际开发中，对象是一个抽象的概念，可以将其简单理解为：数据集或功能集



**什么是面向对象**

- 它只是过程式代码的一种高度封装，目的在于提高代码的开发效率和可维 护性

- 面向对象编程 ------ Object Oriented Programming，简称 OOP ，是一种编程开发思想
  - 在面向对象程序开发思想中，每一个对象都是功能中心，具有明确分工，可以完成接受信息、处理数据、发出信息等任务
  - 面向对象编程具有灵活、代码可复用、高度模块化等特点，容易维护和开发
  - 面向对象的特性：封装性、继承性、\[多态性\]抽象



**程序中面向对象的基本体现**

- 抽象数据行为模板（Class）：

```js
function Student(name, score) {
  this.name = name;
  this.score = score;
  this.printScore = function() {
    console.log('姓名：' + this.name + '  ' + '成绩：' + this.score);
  }
}
```



- 根据模板创建具体实例对象（Instance）：

```js
var std1 = new Student('Michael', 98)
var std2 = new Student('Bob', 81)
```



- 实例对象具有自己的具体行为（给对象发消息）：

```js
std1.printScore() // => 姓名：Michael  成绩：98
std2.printScore() // => 姓名：Bob  成绩 81
```



- 面向对象的设计思想是：
  - 抽象出 Class(构造函数)
  - 根据 Class(构造函数) 创建 Instance
  - 指挥 Instance 得结果



**创建对象**

- 直接通过 \`new Object()\` 创建：

```js
var person = new Object()
person.name = 'Jack'
person.age = 18

person.sayName = function () {
  console.log(this.name)
}
```



- 简写形式 ------ 对象字面量来创建：

```js
var person = {
  name: 'Jack',
  age: 18,
  sayName: function () {
    console.log(this.name)
  }
}
```



- 简单方式的改进：工厂函数

```js
function createPerson (name, age) {
  return {
    name: name,
    age: age,
    sayName: function () {
      console.log(this.name)
    }
  }
}

var p1 = createPerson('Jack', 18)
var p2 = createPerson('Mike', 18)
```



**构造函数**

- 构造函数语法

- 分析构造函数

- 构造函数和实例对象的关系

- 实例的 constructor 属性

- instanceof 操作符

- 普通函数调用和构造函数调用的区别

- 构造函数的返回值

- 构造函数的问题



**更优雅的工厂函数：构造函数**

```js
function Person (name, age) {
  this.name = name
  this.age = age
  this.sayName = function () {
    console.log(this.name)
  }
}

var p1 = new Person('Jack', 18)
p1.sayName() // => Jack

var p2 = new Person('Mike', 23)
p2.sayName() // => Mike
```



- 与基础的工厂函数的区别

  - 没有显示的创建对象
  - 直接将属性和方法赋给了 this 对象
  - 没有 return 语句
  - 函数名使用的是大写的 Person
  - 而要创建 Person 实例，则必须使用 new 操作符

  

- 以这种方式调用构造函数会经历以下 4 个步骤：

  - 创建一个新对象
  - 将构造函数的作用域赋给新对象（因此 this 就指向了这个新对象）
  - 执行构造函数中的代码
  - 返回新对象

  

**构造函数和实例对象的关系**

- 使用构造函数的好处不仅在于代码的简洁性，更重要的是我们可以识别对象的具体类型了

- 在每一个实例对象中同时有一个 \`constructor\` 属性，该属性指向创建该实例的构造函数：

```js
console.log(p1.constructor === Person) // => true
console.log(p2.constructor === Person) // => true
console.log(p1.constructor === p2.constructor) // => true
```



- 对象的 \`constructor\` 属性最初是用来标识对象类型的，但是，如果要检测对象的类型，还是使用 \`instanceof\` 操作符更可靠一些

```js
console.log(p1 instanceof Person) // => true
console.log(p2 instanceof Person) // => true
```



**总结：**

- 构造函数是根据具体的事物抽象出来的抽象模板

- 实例对象是根据抽象的构造函数模板得到的具体实例对象

- 每一个实例对象都具有一个 constructor 属性，指向创建该实例的构造函数
  - 注意： constructor 是实例的属性的说法不严谨，具体后面的原型会讲到

- 可以通过实例的 constructor 属性判断实例和构造函数之间的关系
  - 注意：这种方式不严谨，推荐使用 instanceof 操作符，后面学原型会解释为什么



**构造函数的问题**

- 用构造函数带来最大的好处就是创建对象更方便，但是其本身也存在一个浪费内存的问题：
  - 对于每一个实例对象，属性和方法都是一模一样的内容，每一次生成一个实例，都必须为重复的内容，多占用一些内存，如果实例对象很多，会造成极大的内存浪费。

```js
console.log(p1.sayHello === p2.sayHello) // =\> false
```



- 对于这种问题我们可以把需要共享的函数定义到构造函数外部：

```js
function sayHello = function () {
  console.log('hello ' + this.name)
}

function Person (name, age) {
  this.name = name
  this.age = age
  this.type = 'human'
  this.sayHello = sayHello
}

var p1 = new Person('Top', 18)
var p2 = new Person('Jack', 16)

console.log(p1.sayHello === p2.sayHello) // => true
```



- 但是如果有多个需要共享的函数的话就会造成全局命名空间冲突的问题

```js
//可以把多个函数放到一个对象中用来避免全局命名空间冲突的问题
//至此，基本上解决了构造函数的内存浪费问题
var fns = {
  sayHello: function () {
    console.log('hello ' + this.name)
  },
  sayAge: function () {
    console.log(this.age)
  }
}

function Person (name, age) {
  this.name = name
  this.age = age
  this.type = 'human'
  this.sayHello = fns.sayHello
  this.sayAge = fns.sayAge
}

var p1 = new Person('lpz', 18)
var p2 = new Person('Jack', 16)

console.log(p1.sayHello === p2.sayHello) // => true
console.log(p1.sayAge === p2.sayAge) // => true
```



**原型**

- 使用 prototype 原型对象解决构造函数的问题

- 分析 构造函数、prototype 原型对象、实例对象 三者之间的关系

- 属性成员搜索原则：原型链

- 实例对象读写原型对象中的成员

- 原型对象的简写形式

- 原生对象的原型
  - Object
  - Array
  - String

- 原型对象的问题

- 构造的函数和原型对象使用建议



**更好的解决方案： prototype**

- JavaScript 规定，每一个构造函数都有一个 \`prototype\` 属性，指向另一个对象。这个对象的所有属性和方法，都会被构造函数所拥有。

- 也就意味着，可以把所有对象实例需要共享的属性和方法直接定义在 \`prototype\` 对象上

```js
function Person (name, age) {
  this.name = name
  this.age = age
}

Person.prototype.type = 'human'

Person.prototype.sayName = function () {
  console.log(this.name)
}

var p1 = new Person(...)
var p2 = new Person(...)

console.log(p1.sayName === p2.sayName) // => true
```



**构造函数、实例、原型三者之间的关系**

- 任何函数都具有一个原型 prototype 属性，该属性是一个对象，用来实现基于原型的继承与属性的共享

- 构造函数的 prototype 对象默认都有一个 constructor 属性，指向 prototype 对象所在函数

- 通过构造函数得到的实例对象内部会包含一个指向构造函数的 prototype 对象的指针 \_\_proto\_\_（隐式原型）

- 所有实例都直接或间接继承了原型对象的成员

```js
所有实例都直接或间接继承了原型对象的成员
function Fn(){
    this.a = 10;
    return 1; //返回非null的对象，那么实例化结果就是返回出对象，即fn
    //return {};   //返回{}
}

var f = new Fn();
console.log(f);
```



**属性成员的搜索原则：原型链**

每当代码读取某个对象的某个属性时，都会执行一次搜索，目标是具有给定名字的属性

- 搜索首先从对象实例本身开始

  - 如果在实例中找到了具有给定名字的属性，则返回该属性的值
  - 如果没找到，则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的属性
  - 如果在原型对象中找到了这个属性，则返回该属性的值

  

- 也就是说，在我们调用 person1.sayName() 的时候，会先后执行两次搜索：

  - 首先，解析器会问："实例 person1 有 sayName 属性吗？"答："没有。
  - "然后，它继续搜索，再问：" person1 的原型有 sayName 属性吗？"答："有。
  - "于是，它就读取那个保存在原型对象中的函数。
  - 当我们调用 person2.sayName() 时，将会重现相同的搜索过程，得到相同的结果。而这正是多个对象实例共享原型所保存的属性和方法的基本原理。

  

- 总结：
  - 先在自己身上找，找到即返回
  - 自己身上找不到，则沿着原型链向上查找，找到即返回
  - 如果一直到原型链的末端还没有找到，则返回 undefined



**实例对象读写原型对象成员**

- 读取：按上述的顺序读取



- 值类型成员写入（\`实例对象.值类型成员 = xx\`）：
  - 当实例期望重写原型对象中的某个普通数据成员时实际上会把该成员添加到自己身上
  -  也就是说该行为实际上会屏蔽掉对原型对象成员的访问

  

- 引用类型成员写入（实例对象.引用类型成员 = xx）：
  
  - 同上



- 复杂类型修改（\`实例对象.成员.xx = xx\`）：
  - 同样会先在自己身上找该成员，如果自己身上找到则直接修改
  - 如果自己身上找不到，则沿着原型链继续查找，如果找到则修改
  - 如果一直到原型链的末端还没有找到该成员，则报错（实例对象.undefined.xx = xx）



**更简单的原型语法**

```js
//每添加一个属性和方法就要敲一遍 Person.prototype ,为减少不必要的输入
//用一个包含所有属性和方法的对象字面量来重写整个原型对象：
function Person (name, age) {
  this.name = name
  this.age = age
}

Person.prototype = {
    //不手动添加construct则会丢失该成员
  constructor: Person, // => 手动将 constructor 指向正确的构造函数
  type: 'human',
  sayHello: function () {
    console.log('我叫' + this.name + '，我今年' + this.age + '岁了')
  }
}
```



**原生对象的原型**

------ 所有函数都有 prototype 属性对象

- Object.prototype

- Function.prototype

- Array.prototype

- String.prototype

- Number.prototype

- Date.prototype



**原型对象使用建议**

- 私有成员（一般就是非函数成员）放到构造函数中

- 共享成员（一般就是函数）放到原型对象中

- 如果重置了 prototype 记得修正 constructor 的指向



**继承**

**构造函数的属性继承：借用构造函数**

```js
function Person (name, age) {
  this.type = 'human'
  this.name = name
  this.age = age
}

function Student (name, age) {
  // 借用构造函数继承属性成员 
  Person.call(this, name, age)
}

var s1 = Student('张三', 18)
console.log(s1.type, s1.name, s1.age) // => human 张三 18
```



**构造函数的原型方法继承：拷贝继承（for-in）**

```js
function Person (name, age) {
  this.type = 'human'
  this.name = name
  this.age = age
}

Person.prototype.sayName = function () {
  console.log('hello ' + this.name)
}

function Student (name, age) {
  Person.call(this, name, age)
}

// 原型对象拷贝继承原型对象成员
for(var key in Person.prototype) {
  Student.prototype[key] = Person.prototype[key]
}

var s1 = Student('张三', 18)
s1.sayName() // => hello 张三
```



**另一种继承方式：原型继承**

```js
function Person (name, age) {
  this.type = 'human'
  this.name = name
  this.age = age
}

Person.prototype.sayName = function () {
  console.log('hello ' + this.name)
}

function Student (name, age) {
  Person.call(this, name, age)
}

// 利用原型的特性实现继承
Student.prototype = new Person()

var s1 = Student('张三', 18)
console.log(s1.type) // => human
s1.sayName() // => hello 张三
```



**函数进阶**

**函数的定义方式**

- 函数声明

```js
function foo () {
    //函数体
}
```



- 函数表达式

```js
var foo = function () {
   //函数体
}
```



- new Function



**函数声明与函数表达式的区别**

- 函数声明必须有名字

- 函数声明会函数提升，在预解析阶段就已创建，声明前后都可以调用

- 函数表达式类似于变量赋值

- 函数表达式可以没有名字，例如匿名函数

- 函数表达式没有变量提升，在执行阶段创建，必须在表达式执行之后才可以调用

```js
//以下代码执行结果在不同浏览器中结果不一致。

if (true) {
  function f () {
    console.log(1)
  }
} else {
  function f () {
    console.log(2)
  }
}

//可使用函数表达式解决上面的问题；在if 之前声明 var fn，并在条件内使用函数表达式
```



**函数的调用方式**

- 普通函数

- 构造函数

- 对象方法



**函数内 \`this\` 指向的不同场景**

函数的调用方式决定了 \`this\` 指向的不同：

-------------- ---------------- ------------------------------
  调用方式       		非严格模式          备注
  普通函数调用  	  window               严格模式下是 undefined
  构造函数调用        实例对象              原型方法中 this 也是实例对象
  对象方法调用        该方法所属对象   紧挨着的对象
  事件绑定方法        绑定事件对象     
  定时器函数            window           

-------------- ---------------- ------------------------------



**函数也是对象------call、apply、bind**

--- --- 来帮我们更优雅的处理函数内部 this 指向问题

**call**

```js
fun.call(thisArg[, arg1[, arg2[, ...]]])
```

- call() 方法调用一个函数, 其具有一个指定的 \`this\` 值和分别地提供的参数
  - 注意：该方法的作用和 \`apply()\` 方法类似，只有一个区别，就是 \`call()\` 方法接受的是若干个参数的列表，而 \`apply()\` 方法接受的是一个包含多个参数的数组。

- thisArg
  - 在 fun 函数运行时指定的 this 值
  - 如果指定了 null 或者 undefined 则内部 this 指向 window

- arg1, arg2, \... 指定的参数列表



**apply**

```js
fun.apply(thisArg, [argsArray])
```

- apply()\` 方法调用一个函数, 其具有一个指定的 \`this\` 值，以及作为一个数组
  - 参数：
    - thisArg
    - argsArray



**bind**

```js
fun.bind(thisArg[, arg1[, arg2[, ...]]])
```

- bind() 函数会创建一个新函数（称为绑定函数），新函数与被调函数（绑定函数的目标函数）具有相同的函数体（在 ECMAScript 5 规范中内置的call属性）。

- 当目标函数被调用时 this 值绑定到 bind() 的第一个参数，该参数不能被重写。绑定函数被调用时，bind() 也接受预设的参数提供给原函数。

- 一个绑定函数也能使用new操作符创建对象：这种行为就像把原函数当成构造器。提供的 this 值被忽略，同时调用时的参数被提供给模拟函数。



- 参数：
  - thisArg
    - 当绑定函数被调用时，该参数会作为原函数运行时的 this 指向。当使用new 操作符调用绑定函数时，该参数无效。
  - arg1, arg2, \...
    - 当绑定函数被调用时，这些参数将置于实参之前传递给被绑定的方法

- 返回值：返回由指定的this值和初始化参数改造的原函数拷贝。



示例：

```js
this.x = 9; 
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 返回 81

var retrieveX = module.getX;
retrieveX(); // 返回 9, 在这种情况下，"this"指向全局作用域

// 创建一个新函数，将"this"绑定到module对象
// 新手可能会被全局的x变量和module里的属性x所迷惑
var boundGetX = retrieveX.bind(module);
boundGetX(); // 返回 81
```

```js
function LateBloomer() {
  this.petalCount = Math.ceil(Math.random() * 12) + 1;
}

// Declare bloom after a delay of 1 second
LateBloomer.prototype.bloom = function() {
  window.setTimeout(this.declare.bind(this), 1000);
};

LateBloomer.prototype.declare = function() {
  console.log('I am a beautiful flower with ' +
    this.petalCount + ' petals!');
};

var flower = new LateBloomer();
flower.bloom();  // 一秒钟后, 调用'declare'方法
```



**小结：**

- call 和 apply 特性一样
  - 都是用来调用函数，而且是立即调用
  - 但是可以在调用函数的同时，通过第一个参数指定函数内部 this 的指向
  - call 调用的时候，参数必须以参数列表的形式进行传递，也就是以逗号分隔的方式依次传递即可
  - apply 调用的时候，参数必须是一个数组，然后在执行的时候，会将数组内部的元素一个一个拿出来，与形参一一对应进行传递
  - 如果第一个参数指定了 null 或者 undefined 则内部 this 指向 window



- bind
  - 可以用来指定内部 this 的指向，然后生成一个改变了 this 指向的新的函数
  - 它和 call、apply 最大的区别是：bind 不会调用
  - bind 支持传递参数，它的传参方式比较特殊，一共有两个位置可以传递
    - 在 bind 的同时，以参数列表的形式进行传递
    - 在调用的时候，以参数列表的形式进行传递
    - 那到底以谁 bind 的时候传递的参数为准呢？还是以调用的时候传递的参数为准
    - 两者合并：bind 的时候传递的参数和调用的时候传递的参数会合并到一起，传递到函数内部



**函数的其它成员**

- arguments
  - 实参集合

- caller
  - 函数的调用者

- length
  - 形参的个数

- name
  - 函数的名称

```js
function fn(x, y, z) {
  console.log(fn.length) // => 形参的个数
  console.log(arguments) // 伪数组实参参数集合
  console.log(arguments.callee === fn) // 函数本身
  console.log(fn.caller) // 函数的调用者
  console.log(fn.name) // => 函数的名字
}

function f() {
  fn(10, 20, 30)
}

f()

```



**高阶函数**

------ 函数可以作为参数、 函数可以作为返回值

**作为参数**

```js
function eat (callback) {
  setTimeout(function () {
    console.log('吃完了')
    callback()
  }, 1000)
}

eat(function () {
  console.log('去唱歌')
})
```



**作为返回值**

```js
function genFun (type) {
  return function (obj) {
    return Object.prototype.toString.call(obj) === type
  }
}

var isArray = genFun('[object Array]')
var isObject = genFun('[object Object]')

console.log(isArray([])) // => true
console.log(isArray({})) // => true
```



**函数闭包**

```js
function fn () {
  var count = 0
  return {
    getCount: function () {
      console.log(count)
    },
    setCount: function () {
      count++
    }
  }
}

var fns = fn()

fns.getCount() // => 0
fns.setCount()
fns.getCount() // => 1
```



**JS任务队列**

- JavaScript语言的设计者将所有任务分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）。同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；异步任务指的是，不进入主线程，而进入\"任务队列\"（task queue）的任务，只有\"任务队列\"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。
  - 所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
  - 主线程之外，还存在一个\"任务队列\"（task queue）。只要异步任务有了运行结果，就在\"任务队列\"之中放置一个事件。
  - 一旦\"执行栈\"中的所有同步任务执行完毕，系统就会读取\"任务队列\"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
  - 主线程不断重复上面的第三步



- 哪些语句会放入异步任务队列及放入时机？一般来说，有以下四种会放入异步任务队列：
  - setTimeout和setlnterval
  - DOM事件
  - ES6中的Promise
  - Ajax异步请求



**作用域、作用域链、预解析**

- 全局作用域

- 函数作用域

- 没有块级作用域

------ 内层作用域可以访问外层作用域，反之不行



**什么是闭包**

- 闭包就是能够读取其他函数内部变量的函数，由于在 Javascript 语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成 "定义在一个函数内部的函数"。所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

- 闭包的用途：
  - 可以在函数外部读取函数内部成员
  - 让函数内成员始终存活在内存中



**关于闭包的例子**

```js
//示例1
var arr = [10, 20, 30]
for(var i = 0; i < arr.length; i++) {
  arr[i] = function () {
    console.log(i)
  }
}

//示例2
console.log(111)

for(var i = 0; i < 3; i++) {
  setTimeout(function () {
    console.log(i)
  }, 0)
}
console.log(222)
```



案例思考

```JS
var name = "The Window";
var object = {
  name: "My Object",
  getNameFunc: function () {
    return function () {
      return this.name;
    };
  }
};

console.log(object.getNameFunc()())
```

```JS
var name = "The Window";　　
var object = {　　　　
  name: "My Object",
  getNameFunc: function () {
    var that = this;
    return function () {
      return that.name;
    };
  }
};
console.log(object.getNameFunc()())
```



**函数递归**

**举个栗子：计算阶乘的递归函数**

```JS
function factorial (num) {
  if (num <= 1) {
    return 1
  } else {
    return num * factorial(num - 1)
  }
}
```



**递归应用场景**

- 深拷贝

- 菜单树

- 遍历 DOM 树



**正则表达式**

**什么是正则表达式**

- 正则表达式：用于匹配规律规则的表达式，正则表达式通常被用来检索、替换那些符合某个模式(规则)的文本。

- 正则表达式是对字符串操作的一种逻辑公式，就是用事先定义好的一些特定字符、及这些特定字符的组合，组成一个"规则字符串"，这个"规则字符串"用来表达对字符串的一种过滤逻辑。



**正则表达式的作用**

- 给定的字符串是否符合正则表达式的过滤逻辑(匹配)

- 从字符串中获取我们想要的特定部分(提取)

- 强大的字符串替换能力(替换)



**正则表达式的特点**

- 灵活性、逻辑性和功能性非常的强

- 可以迅速地用极简单的方式达到字符串的复杂控制

- 对于刚接触的人来说，比较晦涩难懂



**正则表达式的测试**

[[在线测试正则]{.underline}](https://c.runoob.com/front-end/854)

- 工具中使用正则表达式
  - sublime/vscode/word
  - 演示替换所有的数字



**正则表达式的组成**

- 普通字符abc 123

- 特殊字符(元字符)：正则表达式中有特殊意义的字符\\d \\w



**常用元字符串**

-------- --------------------------------
  元字符   说明
  \\d      匹配数字
  \\D      匹配任意非数字的字符
  \\w      匹配字母或数字或下划线
  \\W      匹配任意不是字母，数字，下划线
  \\s      匹配任意的空白符
  \\S      匹配任意不是空白符的字符
  .        匹配除换行符以外的任意单个字符
  \^       表示匹配行首的文本(以谁开始)
  \$       表示匹配行尾的文本(以谁结束)
-------- --------------------------------



**限定符**

-------- ------------------
  限定符   说明
  \*       重复零次或更多次
  \+       重复一次或更多次
  ?        重复零次或一次
  {n}      重复n次
  {n,}     重复n次或更多次
  {n,m}    重复n到m次
-------- ------------------



**其它**

- \[\] 字符串用中括号括起来，表示匹配其中的任一字符，相当于或的意思 
- \[\^\] 匹配除中括号以内的内容
-  \\ 转义符
-  \| 或者，选择两者中的一个。注意\|将左右两边分为两部分，而不管左右两边有多长多乱 () 从两个直接量中选择一个，分组 eg：gr(a\|e)y匹配gray和grey
-  \[\\u4e00-\\u9fa5\] 匹配汉字



**案例**

- 验证手机号：

`^\d{11}$`

- 验证邮编：

`^\d{6}$`

- 验证日期 2012-5-01

`^\d{4}-\d{1,2}-\d{1,2}$`

- 验证邮箱 [xxx@itcast.cn](mailto:xxx@itcast.cn)：

` ^\w+@\w+\.\w+$`

- 验证IP地址 192.168.1.10

` ^\d{1,3}\(.\d{1,3}){3}$0`



**使用正则表达式**

- 创建正则对象
  - 方式1：
  
  ```js
  var reg = new RegExp('\d', 'i');
  var reg = new RegExp('\d', 'gi');
  ```
  
  - 方式2：
  
  ```js
  var reg = /\d/i;
var reg = /\d/gi;
  ```

- 参数

------ ---------------------
  标志   说明
  i      忽略大小写
  g      全局匹配
  gi     全局匹配+忽略大小写
------ ---------------------

**正则提取------ str.match()、正则匹配------ reg.test(str)**

- 正则表达式中的()作为分组来使用，获取分组匹配到的结果用Regex.\$1 \$2 \$3\....来获取

```js
// 1. 提取工资
var str = "张三：1000，李四：5000，王五：8000。";
var array = str.match(/\d+/g);
console.log(array);

// 2. 提取email地址
var str = "123123@xx.com,fangfang@valuedopinions.cn 286669312@qq.com 2、emailenglish@emailenglish.englishtown.com 286669312@qq.com...";
var array = str.match(/\w+@\w+\.\w+(\.\w+)?/g);
console.log(array);

// 3. 分组提取  
// 3. 提取日期中的年部分  2015-5-10
var dateStr = '2016-1-5';
// 正则表达式中的()作为分组来使用，获取分组匹配到的结果用Regex.$1 $2 $3....来获取
var reg = /(\d{4})-\d{1,2}-\d{1,2}/;
if (reg.test(dateStr)) {
  //test() 方法执行一个检索，用来查看正则表达式与指定的字符串是否匹配。
  //返回 true 或 false
  console.log(RegExp.$1);
}

// 4. 提取邮件中的每一部分
var reg = /(\w+)@(\w+)\.(\w+)(\.\w+)?/;
var str = "123123@xx.com";
if (reg.test(str)) {
  console.log(RegExp.$1);
  console.log(RegExp.$2);
  console.log(RegExp.$3);
}
```



**正则替换**

```js
// 1. 替换所有空白
var str = "   123AD  asadf   asadfasf  adf ";
str = str.replace(/\s/g,"xx");
console.log(str);

// 2. 替换所有,|，
var str = "abc,efg,123，abc,123，a";
str = str.replace(/,|，/g, ".");
console.log(str);
```



**案例：表单验证**

```html
QQ号：<input type="text" id="txtQQ"><span></span><br>
邮箱：<input type="text" id="txtEMail"><span></span><br>
手机：<input type="text" id="txtPhone"><span></span><br>
生日：<input type="text" id="txtBirthday"><span></span><br>
姓名：<input type="text" id="txtName"><span></span><br>
```

```js
//获取文本框
var txtQQ = document.getElementById("txtQQ");
var txtEMail = document.getElementById("txtEMail");
var txtPhone = document.getElementById("txtPhone");
var txtBirthday = document.getElementById("txtBirthday");
var txtName = document.getElementById("txtName");

//
txtQQ.onblur = function () {
  //获取当前文本框对应的span
  var span = this.nextElementSibling;
  var reg = /^\d{5,12}$/;
  //判断验证是否成功
  if(!reg.test(this.value) ){
    //验证不成功
    span.innerText = "请输入正确的QQ号";
    span.style.color = "red";
  }else{
    //验证成功
    span.innerText = "";
    span.style.color = "";
  }
};

//txtEMail
txtEMail.onblur = function () {
  //获取当前文本框对应的span
  var span = this.nextElementSibling;
  var reg = /^\w+@\w+\.\w+(\.\w+)?$/;
  //判断验证是否成功
  if(!reg.test(this.value) ){
    //验证不成功
    span.innerText = "请输入正确的EMail地址";
    span.style.color = "red";
  }else{
    //验证成功
    span.innerText = "";
    span.style.color = "";
  }
};
```



- 表单验证部分，封装成函数：

```js
var regBirthday = /^\d{4}-\d{1,2}-\d{1,2}$/;
addCheck(txtBirthday, regBirthday, "请输入正确的出生日期");
//给文本框添加验证
function addCheck(element, reg, tip) {
 element.onblur = function () {
   //获取当前文本框对应的span
   var span = this.nextElementSibling;
   //判断验证是否成功
   if(!reg.test(this.value) ){
     //验证不成功
     span.innerText = tip;
     span.style.color = "red";
  }else{
     //验证成功
     span.innerText = "";
     span.style.color = "";
  }
};
}
```



- 通过给元素增加自定义验证属性对表单进行验证

```html
<form id="frm">
  QQ号：<input type="text" name="txtQQ" data-rule="qq"><span></span><br>
  邮箱：<input type="text" name="txtEMail" data-rule="email"><span></span><br>
  手机：<input type="text" name="txtPhone" data-rule="phone"><span></span><br>
  生日：<input type="text" name="txtBirthday" data-rule="date"><span></span><br>
  姓名：<input type="text" name="txtName" data-rule="cn"><span></span><br>
</form>
```

```js
// 所有的验证规则
var rules = [
  {
    name: 'qq',
    reg: /^\d{5,12}$/,
    tip: "请输入正确的QQ"
  },
  {
    name: 'email',
    reg: /^\w+@\w+\.\w+(\.\w+)?$/,
    tip: "请输入正确的邮箱地址"
  },
  {
    name: 'phone',
    reg: /^\d{11}$/,
    tip: "请输入正确的手机号码"
  },
  {
    name: 'date',
    reg: /^\d{4}-\d{1,2}-\d{1,2}$/,
    tip: "请输入正确的出生日期"
  },
  {
    name: 'cn',
    reg: /^[\u4e00-\u9fa5]{2,4}$/,
    tip: "请输入正确的姓名"
  }];

addCheck('frm');


//给文本框添加验证
function addCheck(formId) {
  var i = 0,
      len = 0,
      frm =document.getElementById(formId);
  len = frm.children.length;
  for (; i < len; i++) {
    var element = frm.children[i];
    // 表单元素中有name属性的元素添加验证
    if (element.name) {
      element.onblur = function () {
        // 使用dataset获取data-自定义属性的值
        var ruleName = this.dataset.rule;
        var rule =getRuleByRuleName(rules, ruleName);

        var span = this.nextElementSibling;
        //判断验证是否成功
        if(!rule.reg.test(this.value) ){
          //验证不成功
          span.innerText = rule.tip;
          span.style.color = "red";
        }else{
          //验证成功
          span.innerText = "";
          span.style.color = "";
        }
      }
    }
  }
}

// 根据规则的名称获取规则对象
function getRuleByRuleName(rules, ruleName) {
  var i = 0,
      len = rules.length;
  var rule = null;
  for (; i < len; i++) {
    if (rules[i].name == ruleName) {
      rule = rules[i];
      break;
    }
  }
  return rule;
}
```



**附录**

**代码规范**

- **代码风格**
  - [JavaScript Standard Style ](https://github.com/feross/standard)
  - [Airbnb JavaScript Style Guide()](https://github.com/airbnb/javascript)

- **校验工具**
  - [JSLint](https://github.com/douglascrockford/JSLint)
  - [JSHint](https://github.com/jshint/jshint)
  - [ESLint](https://github.com/eslint/eslint)

- JSLint 通过检查和分析 JavaScript 代码，将任何违反规则的代码警告给开发者，且无法通过配置关闭一些开发者认为不是问题的警告，而导致检查和开发无法继续下去。

- JSHint与 JSLint 具有相同用途的 JavaScript 静态代码分析工具，**JSHint 是在 JSLint 代码基础上二次开发而来的。**JSHint 设计得非常可配置，提供了丰富的指令和选项，可根据开发者以及研发团队的自身需要调整JSHint符合自己的编码规则、风格和品位。

- ESLint最初并不是为了造一个重复的轮子，而是作者在实际使用中的需求[没有能得到JSHint团队的回应](https://github.com/eslint/eslint#why-dont-you-like-jshint)，所以他就结合当时的JSHint和另一个代码风格的检查工具**JSCS**写出来了现在具备**代码风格检查**，**自定义插件扩展**功能的ESLint了。



Chrome 开发者工具

文档相关工具

电子文档制作工具: [docute](https://github.com/egoist/docute)

流程图工具：[DiagramDesigner](http://logicnet.dk/DiagramDesigner/)
