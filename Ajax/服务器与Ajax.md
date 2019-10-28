### 服务器与Ajax

> Ajax基本使用

- 显示新的HTML内容而不用载入整个页面
- 提交一个表单并且立即显示结果
- 登录而不用跳转到新的页面
- 星级评定组件
- 遍历数据库信息加载更多而不刷新页面



> 异步加载

- iframe实现无刷新页面加载效果
- 原生ajax实例实现异步加载效果



#### 原生Ajax

> 原生Ajax的请求步骤

- 创建XMLHttpRequest对象
- 准备发送
- 执行发送动作
- 执行回调函数



> Ajax请求方法

- get：请求参数在url中传递（需要注意编码）
- post：请求参数在请求体中传递（需要设置请求头）



> 原生Ajax详解-回调函数

- onreadystatechange  （每当 readyState 改变时，就会触发 onreadystatechange 事件）
- xhr.readyState
  - 0  xhr对象初始化
  - 1  执行发送动作
  - 2  服务器端数据已经完全返回
  - 3  数据正在解析
  - 4  数据解析完成，可以使用了
- xhr.status
  - 200 数据相应正常
  - 404 没有找到资源
  - 500 服务器端错误



> 原生Ajax返回数据

- xml
- json
- responseXML
- responseText



> 异步效果与js事件处理机制

- 定时函数
  setTimeout()
  setInterval()
- 事件函数
  btn.onclick=functoin(){}
- Ajax回调函数
  xhr.onreadystatechange=function()



#### jQuery的Ajax

> jQuery的使用

- $.ajax()
- 属性
  - jsonp
  - jsonpCallback
- 常见的使用：搜索智能、快递查询



> Ajax的跨域

- 同源策略
  - 同源策略是浏览器的一种安全策略，所谓同源指的是请求URL地址中的**协议、域名和端口**都相同，只要其中之一不相同就是跨域
  - 同源策略主要为了保证浏览器的安全性
  - 在同源策略下，浏览器不允许Ajax跨域获取服务器数据



> 跨域解决方案

- jsonp
  - 静态script标签的src属性进行跨域请求
  - 动态创建script标签，通过标签的src属性发送请求
- document.domain+iframe
- location.hash + iframe
- window.name + iframe
- window.postMessage 
- flash等第三方插件



> 模板引擎

- 模板+数据 = 静态页面片段
- 使用步骤（art-template）
  - 引入template.js文件
  - 定义模板
  - 将数据和模板结合起来生成html片段
  - 将html片段渲染到界面中

``` html
<head>
    <meta charset="UTF-8">
    <title>artTemplate</title>
    <script src="template.js"></script>
    <!--定义模板-->
    <script type="text/html" id="resultTemplate">
            <h1>{{title}}</h1>
            <ul>
            {{each books as value i}}
                <li>{{value}}</li>
            {{/each}}
        </ul>
    </script>
    <script>
        window.onload = function(){
            var data = {
                title:"四大名著图书信息",
                books:['西游记','水浒传','三国演义','红楼梦']
            };
            //生成html片段
            var html = template("resultTemplate",data);
            var container = document.getElementById("container");
            //将html片段渲染到界面中
            container.innerHTML = html;
        };
    </script>
</head>
```





