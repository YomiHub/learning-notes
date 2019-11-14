### CSS书写规范

> 空格规范

- 选择器与 { 之间必须包含空格【强制】
  示例：`.selector {}`

- 属性名与之后的冒号：之间不允许有空格，冒号：与属性值之间必须包含空格【强制】
  - 示例：fontsize: 14px;



> 选择器规范

- 当一个rule 包含多个 selector 时，每个选择器声明必须独占一行 【强制】

  ```CSS
  .post,
  .page,
  .comment {
      line-height: 1.5;
  }
  ```

  

- 选择器的嵌套层级应不大于 3 级，位置靠后的限定条件应尽可能精确【建议】

  ```CSS
  #username input {}
  .comment .avatar {}
  ```

  

> 属性规范

- 属性定义必须另起一行 【强制】

  ```CSS
  .selector {
      margin: 0; /* 换行 */
      padding: 0;
  }
  ```



- 属性定义后必须以分号结尾
  示例：margin: 0; 分号不能省略