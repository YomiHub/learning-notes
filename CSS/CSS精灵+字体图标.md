### CSS精灵+字体图标

#### CSS精灵技术（sprite)

> 当用户访问一个网站时，需要向服务器发送请求，网页上的每张图像都要经过一次请求才能展现给用户
>
> 当网页中的图像过多时，服务器就会频繁地接受和发送请求，这将大大降低页面的加载速度。为了有效地减少服务器接受和发送请求的次数，提高页面的加载速度，出现了CSS精灵技术（也称CSS Sprites、CSS雪碧）



**精灵技术本质**

- CSS精灵是一种处理网页背景图像的方式。它将一个页面涉及到的所有零星背景图像都集中到一张大图中去，然后将大图应用于网页，这样，当用户访问该页面时，只需向服务发送一次请求，网页中的背景图像即可全部展示出来。通常情况下，这个由很多小的背景图像合成的大图被称为精灵图（雪碧图）
- 精灵技术使用：需要使用CSS的background-image、background-repeat和background-position属性进行背景定位



**制作精灵图**

- 放的都是小的装饰性质的背景图片。 插入图片不能往上放
- 宽度取决于最宽的那个背景
- 可以横向摆放也可以纵向摆放，但是每个图片之间，间隔至少隔开偶数像素合适。
- 精灵图的最底端，留一片空隙，方便我们以后添加其他精灵图



**字体图标**

- 图片不但增加了总文件的大小，还增加了很多额外的"http请求"，这都会大大降低网页的性能的。更重要的是图片不能很好的进行“缩放”，因为图片放大和缩小会失真。而且很多情况下希望我们的图标是可以缩放的，这时候字体图标的作用就很重要



- 优点：

- - 做出跟图片一样可以做的事情,改变透明度、旋转度，等
  - 本质其实是文字，可以很随意的改变颜色、产生阴影、透明效果
  - 本身体积更小，但携带的信息并没有削减
  - 几乎支持所有的浏览器
  - 移动端设备必备良药



**字体图标使用流程**

1. UI人员设计字体图标效果图（SVG）——一般在illustractor、Sketch这类矢量图形软件创建
2. 前端人员上传生成兼容性字体文件包
3. 前端人员下载兼容字体文件包到本地
4. 把字体文件包引入到HTML页面



**上传生成字体包**

- 当UI设计人员给我们svg文件的时候，我们需要转换成我们页面能使用的字体文件， 而且需要生成的是兼容性的适合各个浏览器的。
- 推荐网站： http://icomoon.io



**icomoon字库**

- IcoMoon成立于2011年，推出的第一个**自定义图标字体生成器**，允许用户选择他们所需要的图标，使它们成一字型。 内容种类繁多，非常全面，唯一的遗憾是国外服务器，打开网速较慢。
-    推荐网站： http://www.iconfont.cn/



**阿里icon font字库**

- 这个是阿里妈妈M2UX的一个icon font字体图标字库，包含了淘宝图标库和阿里妈妈图标库。可以使用AI制作图标上传生成。 一个字，免费，免费！！
- http://www.iconfont.cn/



**fontello**

- http://fontello.com/
- 在线定制你自己的icon font字体图标字库，也可以直接从GitHub下载整个图标集，该项目也是开源的。



**Font-Awesome**

- http://fortawesome.github.io/Font-Awesome/



**Glyphicon Halflings**

- http://glyphicons.com/
- 这个字体图标可以在Bootstrap下免费使用。自带了200多个图标。



**Icons8**

- https://icons8.com/
- 提供PNG免费下载，像素大能到500PX



**下载兼容字体包**

- 网站会给我们把UI做的svg图片转换为我们的字体格式， 然后下载下来就好了（4个文件）



**将字体引入到HTML**

- 首先把 下载的4个文件放入到 fonts文件夹里面（.eot、.svg、.ttf、.woff）
- 在样式里面声明字体： 告诉别人我们自己定义的字体

```css
@font-face {
  font-family: 'icomoon';
  src:  url('fonts/icomoon.eot?7kkyc2');
  src:  url('fonts/icomoon.eot?7kkyc2#iefix') format('embedded-opentype'),
    url('fonts/icomoon.ttf?7kkyc2') format('truetype'),
    url('fonts/icomoon.woff?7kkyc2') format('woff'),
    url('fonts/icomoon.svg?7kkyc2#icomoon') format('svg');
  font-weight: normal;
  font-style: normal;
}
```

- 给盒子使用字体

```css
span {     font-family: "icomoon"; }
```



- 盒子里面添加结构

```html
span::before {
		 content: "\e900";
	}
或者  
<span></span>  /* 符号快捷键：ctrl+shift+v*/
```



**追加新图标到原来库里面**

- 需要添加新的字体图标，但是原来的不能删除，继续使用，此时只要把压缩包里面的selection.json 重新上传，然后，选中自己想要新的图标，从新下载压缩包，替换原来文件即可。
- 提示：在”导入图标“按钮中选择json文件导入