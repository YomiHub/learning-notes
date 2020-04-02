### gulp介绍与基本使用

#### gulp介绍
- 官网[地址]("https://gulpjs.com/")，gulp相关插件查找[链接]("https://gulpjs.com/plugins/")
- 与grunt功能类似的前端项目构建工具，也是基于node.js的自动任务运行器：将机械化操作编写成任务，想要执行机械化操作时执行一个命令行命令，任务就能自动执行了
- 能自动化完成 javascript、coffe、sass、less、html、image、css等文件的合并、压缩、检查、公共文件抽离、监听文件变化、浏览器自动刷新、测试任务等
- gulp更高效（异步多任务）、更易于使用、插件高质量
- gulp的基本项目结构
```
/- dist
/- src
  /- css
  /- less
  /- js
/- index.html
/- gulpfile.js
/- pakage.json
```

<br>

#### 安装gulp
- 局部安装：`npm install --save-dev gulp`，安装完成后可以通过`gulp --version`查看版本
- gulp提供了命令行工具：gulp-cli，可以通过命令`npm i gulp-cli -g`安装

<br>

#### Gulp中常用的方法
- gulp.src():获取任务要处理的文件，通过第一个参数即任务名区分任务
- gulp.dest():输出文件
- gulp.task():简历gulp任务
- gulp.watch():监控文件的变化

```js
const gulp = require('gulp');
//使用gulp.task()建立任务
gulp.task('first', () => {
  //获取要处理的文件：带文件名称
  gulp.src('./src/css/reset.css')  
    .pipe(gulp.dest('./dist/css'));  //相当于拷贝了该reset.css
});

//可以使用gulp-cli执行gulp任务：`gulp first`
```

<br>

#### 常用的Gulp插件
- gulp-htmlmin:html文件压缩
- gulp-csso:压缩css
- gulp-babel:Javascript语法转换
- gulp-less:less语法转换
- gulp-uglify:压缩混淆Javascript
- gulp-concat  合并文件`return gulp.src('src/js/**/*.js').pipe(concat(build.js)).pipe(gulp.dest('dist/js'))`;
- gulp-imagemin    压缩图片
- gulp-clean   清空文件夹
- gulp-clean-css `return gulp.src('./src/css/*.css').pipe(cssClean({compatibility:'ie8'})).pipe(gulp.dest('./dist/css'))`
- gulp-file-include:公共文件包括
- browsersync:浏览器实时同步
- gulp-livereload  自动编译

<br>

#### 插件使用案例：gulp-htmlmin、gulp-file-include
- 使用`npm i -D gulp-htmlmin` 安装html文件压缩插件https://www.npmjs.com/package/gulp-htmlmin
- 使用命令`npm i D gulp-file-include` 安装公共文件包括https://www.npmjs.com/package/gulp-file-include
  + 可以将页面公共部分提取到单个文件放到common文件夹中（比如页面的header和footer）
  + 使用gulp-file-include将公共部分引入到具体页面

```html
<!--需要引入公共模块的页面-->
<!DOCTYPE html>
<html>
  <body>
  @@include('./var.html', {
    "name": "haoxin",
    "age": 12345,
    "socials": {
      "fb": "facebook.com/include",
      "tw": "twitter.com/include"
    }
  })
  </body>
</html>

<!--公共模块var.html-->
<label>@@name</label>
<label>@@age</label>
<strong>@@socials.fb</strong>
<strong>@@socials.tw</strong>
```
- css任务：less语法转换、css代码压缩
  + 安装插件：`npm i gulp-less gulp-csso -D`

- js任务：babel编译、压缩合并代码
  + 安装babel7：`npm install --save-dev gulp-babel @babel/core @babel/preset-env`
  + 安装gulp-uglify：`npm install --save-dev gulp-uglify`

- 拷贝所有静态文件所在的文件夹
  + 例如images文件夹、lib文件夹

```js
//gulpfile.js
const gulp = require('gulp');
const htmlmin = require('gulp-htmlmin');
const fileinclude = require('gulp-file-include');
const less = require('gulp-less');
const csso = require('gulp-csso');
const babel = require('gulp-babel');
const uglify = require('gulp-uglify');


//使用gulp-htmlmin插件压缩文件 https://www.npmjs.com/package/gulp-htmlmin
gulp.task('htmlmin', (done) => {
  gulp.src('./src/view/*.html')   //压缩view文件夹下所有的html文件
    .pipe(fileinclude({
      prefix: '@@',
      basepath: '@file'
    }))
    .pipe(htmlmin({ collapseWhitespace: true }))  //压缩空格
    .pipe(gulp.dest('./dist/view'));
  done();//task完成时通知Gulp（而不是返回一个流）或者使用async···await
})

//使用gulp-less编译less：https://www.npmjs.com/package/gulp-less#using-plugins
//使用gulp-csso压缩:https://www.npmjs.com/package/gulp-csso
gulp.task('cssmin', async () => {
  await gulp.src(['./src/less/*.less', './src/css/*css'])
    .pipe(less()) //将less代码转为css
    .pipe(csso())
    .pipe(gulp.dest('./dist/css'));
})

//使用gulp-babel转为浏览器兼容的js：https://www.npmjs.com/package/gulp-babel
//使用gulp-uglify压缩代码
gulp.task('jsmin', () => {
  return gulp.src('./src/js/*.js')
    .pipe(babel({
      presets: ['@babel/env']
    }))
    .pipe(uglify())   //压缩JS
    .pipe(gulp.dest('./dist/js'));//使用return 代替done()、async
})

//拷贝静态文件所在的文件夹
gulp.task('copy', () => {
  return gulp.src(['./src/imgs/*', './src/imgs/**/*'])  //**两个通配符表示深度遍历
    .pipe(gulp.dest('./dist/imgs'));  //拷贝图片
})

//构建任务
gulp.task('default', gulp.series(['htmlmin', 'cssmin', 'jsmin', 'copy'])); //通过执行gulp default或者gulp依次执行任务

```

<br>

#### 自动化构建
- 半自动化构建watch
  + 自动编译gulp-livereload `npm i gulp-livereload -D`
  + 自动打开浏览器`npm i gulp-open -D`

```js
const livereload = require('gulp-livereload');
//注册监视任务Gulp.watch(“监听的文件”，回调函数)。
gulp.task('watch', async () => {
  //开启监听
  livereload.listen();
  //确认监听的目标以及绑定相应的任务，在对应task中必须实时刷新.pipe(livereload())
  gulp.watch('./src/js/*.js', async () => {
    //4.0+ jsmin任务写在这里
    gulp.src('./src/js/*.js')
      .pipe(babel({
        presets: ['@babel/env']
      }))
      .pipe(uglify())   //压缩JS
      .pipe(gulp.dest('./dist/js'))
      .pipe(livereload());  //实时刷新
  });
})

//gulp watch运行
```
- 全自动化gulp-connect
  + 安装`npm i gulp-connect -D`
  + 安装gulp-open自动打开指定链接`npm i open -D`

```js
const connect = require('gulp-connect');
const open = require('open');

//全自动监视任务gulp-connect（browserSyn）
gulp.task('connect', async () => {
  connect.server({
    root: './dist',
    livereload: true,  //实时刷新
    port: 5000  //配置端口号
  });

  gulp.watch('./src/css/*.css', async () => {
    //4.0+ jsmin任务写在这里
    gulp.src(['./src/less/*.less', './src/css/*css'])
      .pipe(less()) //将less代码转为css
      .pipe(csso())
      .pipe(gulp.dest('./dist/css'))
      .pipe(connect.reload());  //实时刷新
  });

  open('http://localhost:5000');  //自动打开指定链接
})
//运行gulp connect 自动打开页面http://localhost:5000

```

