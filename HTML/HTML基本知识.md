[HTML基本知识](#1155-1569069397322)

[HTML标签](#38gfnx1567392260199)



### HTML基本知识

**head标签**

- 作用：用于存放title,meta,base,style,script,link、
- 注意：在head标签中必须要设置的标签是title



**文档类型<!DOCTYPE>**

`<!DOCTYPE html> `

- 作用：告诉用户和浏览器使用的是html5版本，向浏览器说明当前文档使用哪种 HTML 或 XHTML 标准规范
- 注意：一些老网站可能用的还是老版本的文档类型比如 XHTML之类的，但HTML5的文档类型兼容很好(向下兼容的原则)，所以可以放心使用HTML5的文档类型



**字符集**

`<meta charset="UTF-8">`

- utf-8是目前最常用的字符集编码方式，常用的字符集编码方式还有gbk和gb2312
- gb2312 简单中文  包括6763个汉字
- BIG5   繁体中文 港澳台等用
- GBK包含全部中文字符    是GB2312的扩展，加入对繁体字的支持，兼容GB2312
- UTF-8则包含全世界所有国家需要用到的字符



**HTML标签的语义化**

- 作用：
  - 方便代码的阅读和维护
  - 让浏览器或网络爬虫更好地解析，从而更好分析其中的内容 
  - 会具有更好地搜索引擎优化
- 检查语义是否良好：去掉CSS之后，网页结构依然组织有序，并且有良好的可读性



### HTML标签

**标题标签**

-  注意：h1 标签因为重要，尽量少用，不要轻易使用h1，一般h1 都是给logo使用的



**水平线标签**

```
<hr /> <!-- 单词缩写：  horizontal  横线，这个水平线也可以通过插入图片实现 --> 
```



**div span标签**

- div  span    是没有语义的，同时也是网页布局主要的2个盒子



**文本格式化标签**

- b  i  s  u   只有使用 没有 强调的意思       strong   em  del   ins  语义更强烈

| 标签                       | 效果                           |
| -------------------------- | ------------------------------ |
| <b></b>  <strong></strong> | 粗体（XHTML推荐使用strong）    |
| <i></i>  <em></em>         | 斜体（XHTML推荐使用em）        |
| <s></s>  <del></del>       | 添加删除线（XHTML推荐使用del） |
| <u></u>  <ins></ins>       | 添加下划线（XHTML不赞成使用u） |



**标签属性**

- 标签可以拥有多个属性，必须写在开始标签中，位于标签名后面

- 属性之间不分先后顺序，标签名与属性、属性与属性之间均以空格分开（采取  键值对  的格式   key="value"  的格式）

- 任何标签的属性都有默认值，省略该属性则取默认值

  <div width="400"></div>
**图像标签<img />**

- 标记属性有src、alt（图像不能显示时的替换文本）、title（鼠标悬停显示的内容）、border（图像边框的宽度）、width（XHTML不支持百分比）、height（XHTML不支持百分比）
- 注意：其中src属性是必需属性



**链接标签<a></a>**

`<a href="超链接地址" target="目标窗口弹出方式：self|blank">`

- 注意：外部链接需要添加http://，且没有确定链接目标时通常将href定义为“#”



**锚点**

- 第一步：使用`<a href="#id名>“链接文本"</a>`创建链接文本
- 第二步：在跳转目标位置使用对应的id名标注



**base标签<base />**

- 作用：用于设置整个页面中链接的打开状态
- 标签位置：写在`<head></head>`里面

`<head> <base target="blank" /> </head> `



**无序列表ul、有序列表ol**

- `<ul></ul>`和`<ol></ol>`中只能嵌套`<li></li>`，直接在列表标签中输入其他标签或者文字的做法是不被允许的
- <li>与</li>之间相当于一个容器，可以容纳所有元素
- 无序（有序）列表会带有自己样式属性



**自定义列表**

- 定义列表常用于对术语或名词进行解释和描述，定义列表的列表项前没有任何项目符号（一般用于网站最下方，放有类似相关关系的跳转链接，dd 一般都是围绕dt来解释的，比如说市级和区之间的从属关系）

```
<!-- dl、dt、dd都是块级元素 -->
<dl>
  <dt>名词1</dt>
  <dd>名词1解释1</dd>
  <dd>名词1解释2</dd>
  ...
  <dt>名词2</dt>
  <dd>名词2解释1</dd>
  <dd>名词2解释2</dd>
  ...
</dl>
```



**创建表格**

- <tr></tr>中只能嵌套`<td></td>`或`<th></th>`
-  <td></td>标签像一个容器，可以容纳所有的元素
- 头部、主体和页脚对应的三个标签是:<thead></thead>、<tbody></tbody>、<tfoot></tfoot>

``` <table>
<table>
  <tr>
    <th>表头</th>
  </tr>
  <tr>
    <td>单元格内容</td>
  </tr>
</table>
```



**表格属性**

- 表格属性有：border（默认为0）、cellspacing（单元格与单元格边框间的间距，默认为2px）、cellpadding（单元格内容与单元格边框之间的间距，默认为1px）、width、height、align（表格在网页中水平对齐方式：left、center、right）
- 注意：现在普遍使用border-collapse: separate/collapse



**合并单元格**

- 跨行合并：rowspan    跨列合并：colspan

``` <table border = "1">
<table>
  <tr>
        <td rowspan = "2">Row 1 Cell 1</td>
        <td>Row 1 Cell 2</td>
        <td>Row 1 Cell 3</td>
   </tr>
    <tr>
        <td>Row 2 Cell 2</td>
        <td>Row 2 Cell 3</td>
   </tr>
   <tr>
        <td colspan = "3">Row 3 Cell 1</td>
    </tr>
 </table>

```



**input控件 <input />**

| 属性                         | 属性值                                       | 描述                                              |
| ---------------------------- | -------------------------------------------- | ------------------------------------------------- |
| type                         | text、password                               | 单行文本输入框、密码输入框                        |
| radio、checkbox              | 单选按钮、复选框                             |                                                   |
| button、submit、reset、image | 普通按钮、提交按钮、重置按钮、图片式提交按钮 |                                                   |
| file                         | 文件域                                       |                                                   |
| name、value                  | 用户自定义                                   | 控件名称、控件默认文本                            |
| size、maxlength              | 正整数                                       | 控件在页面中显示的宽度、 控件允许输入的最多字符数 |
| checked                      | checked                                      | 定义选择控件默认选中项                            |

- 可联合<label  for =“id 名”>使用



**textarea控件（文本域）**

``` <textarea cols="每行中的字符数" rows="显示的行数">     文本内容，可以自动换行 </textarea> ```



**下拉菜单**

``` <select></select>中至少应包含一对<option></option>
<select>
  <option selected =" selected ">选项1</option>
  <option>选项2</option>
</select>
```