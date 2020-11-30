### Cavans

> 非标准IE不兼容

```html
<canvas>
    <span>当不兼容canvas的时候就会显示该标签中的内容</span>
</canvas>
```

---

#### 绘图

>  绘制方块

- fillRect(L,T,W,H)  :默认颜色是黑色

- strokeRect(L,T,W,H)   :带边框的方块

- - 默认一像素黑色边框，显示出来不一样的原因(绘图时从绘图点向左右延伸0.5像素)

```JS
var oGC = oC.getContext('2d'); 
oGc.fillRect(left,top,width,height) 
```



> 设置绘图

- fillStyle: 填充颜色（绘制canvas是有顺序的）  oGC.fillStyle = 'blue';
- lineWidth: 线宽度，是一个数值   oGC.lineWidth = 10;
- strokeStyle: 边线颜色



> 边界绘制

- lineJoin : 边界连接点的样式

- - miter（默认）、round（圆角）、bevel（斜角）

- lineCap:端点样式

- - butt（默认）、round（圆角）、square（高度多出为宽一半的值）



> 绘制路径

- beginPath: 开始绘制路径
- closePath: 结束绘制路径
- moveTo: 移动到绘制的新目标（确定绘制的起点）
- lineTo: 新的目标点  （要用stroke()描绘路径才会有效果）
- fill()



> 绘制圆

- arc(x,y,半径，起始弧度，结束弧度，旋转方向)

- - 弧度 = 角度*Math.PI/180
  - 旋转方向：顺时针（默认false）、逆时针（true）

- oGC.stroke();  绘制之后才会显示



> 画布刷新

- 清空画布：oGC.clearRect(0,0,ele.width,ele.height)



> canvas变换

- translate

- - 偏移：以起始点为基准点，移动当前坐标（即改变0,0点的位置）

- rotate

- - 旋转：参数为弧度（以起点为中心点旋转）

- scale

- - 缩放



---

#### 插图

> canvas插入图片

- 在图片加载完之后调用方法`drawImage(oIg,x,y,w,h)`

```
window.onload = function(){
    var ele = document.getElementById("canvasId");
    var oGC = ele.getContext("2d");
    
    var yImg = new Image();
    
    yImg.onload = function(){
        draw(this);
    };
    
    yImg.src = '2.png';
    
    function draw(obj){
        oGC.drawImage(obj,0,0,400,400)   //图片对象、图片位置、图片大小
    }
} 
```



> cavans设置背景 

- createPattern(obj,平铺方式)

- - repeat、repeat-x、repeat-y、no-repeat

```js
var bg = oGC.createPattern(obj,'repeat')
oGC.fillStyle = bg;
oGC.fillRect(0,0,300,300)
```



> canvas渐变

- createLinearGradient(x1,y1,x2,y2)  线性渐变
- createRadialGradient(x1,y1,r1,x2,y2,r2)  放射性渐变

```js
var obj = OGC.createLinearGradient(150,100,150,200); //渐变起点和终点
obj.addColorStop(0,'red');
obj.addColorStop(1,'blue');   //可以在0-1之间填充多个点

oGC.fillStyle = obj;   //填充渐变
oGC.fillRect(150,100,100,100);   //绘制正方形
```



#### 文字

> 文字效果

```js
oGC.font = '60px impact'   //字体大小，字体样式
oGC.textBaseline = 'top'    //垂直对齐方向

OGC.shadowOffsetX = 10;  //阴影偏移
OGC.shadowOffsetY = 10;
oGC.shadowBlur =3;   //模糊
oGC.shadowColor = 'yellow';   //不设置就无法显示阴影
  
oGC.measureText(str).width   //获取到文字的宽，没有高度
oGC.fillText('这是填充的文字',0,0);
oGC.strokeText('文字',0,200);     //描线文字，镂空效果
```



#### 像素

> 获取图像数据`getImageData(x,y,w,h)`、设置图像数据`putImageData(获取图像,x,y)`

- 属性：

- - width: 一行的像素个数
  - height:一列的像素个数
  - data: 整体像素的数组集合 data.length  像素值的个数，4个值组成一个像素集合rgba

```js
var oImg = oGC.getImageData(0,0,100,100);   //起点，宽高
//中间可以设置oImg的属性： OImg.data[0] = 255;
oGC.putImageData(oImg,100,100)  //将获取到的像素填到新的位置
```



> 创建像素

- var oImg = oGC.createImageData(100,100);   //宽高



> 获取或者设置坐标值的像素点

```js
function getXY(oImg,x,y){   //图像数据，坐标x、y
    var w = oImg.width;
    var h = oImg.height;
    var d = oImg.data;
    
    var color=[];
    color[0] = d[4*(y*w+x)]
    color[1] = d[4*(y*w+x)+1]
    color[2] = d[4*(y*w+x)+2]
    color[3] = d[4*(y*w+x)+3]
    return color;
}
```



> 图片的像素操作

- 图片必须是同源的，即同一个域名或者下载到本地



#### canvas合成

>  全局阿尔法值

- globalAlpha

```oGC.globalApha = 0.5;    //后面叠加的图形都是半透明 ```



> 覆盖合成

- 源：新的图形

- 目标：已经绘制过的图形

- - globalCompositeOperation属性

```oGC.globalCompositeOperation = 'source-atop'  //截取公共部分（取交集） ```



#### 将画布导出为图像

> toDataURL

- 火狐右键可以直接导出成图片

```js
ele.toDataURL()   //ele = document.getElemnetById('canvas');

oImg.src = ele.toDataURL()
```



> 事件操作

- isPointInPath(x,y)

- - 是否在点击范围内

  - jCanvaScript(相当于说是canvas中的jquery)：

  - - http://blog.karachicorner.com/2011/03/html5-canvas-manage-javascrit-library-jcanvascript/

```js
ele.onmousedown = fucntion(ev){
    var ev = ev ||window.event;
    var x = ev.clientX-ele.offsetLeft;
    var y = ev.clientY-ele.offsetTop;
    
    if(oGC.isPointPath(x,y)){
        //点击了最后一次的绘图部分
    }
}
```



- 构造函数：解决多个图形的点击事件

```js
var oC = documentById('c1')
var oGDC = oC.getContext('2d');

var c1 = new Shape(100,100,50)
var c2 = new Shape(200,200,50)

oGC.onmousedown = function(ev){
    var ev = ev||window.event;
    var point = {
        x:ev.clientX-oC.offsetLeft;
        y:ev.clientY-oC.offsetTop; 
    }
    
    c1.reDraw(point);
    c2.redraw(point);
}
c1.click = functino(){
    alert(123);
};

c2.click = function(){
    alert(456);
};

function Shape(x,y,r){
    this.x = x;
    this.y = y;
    this.r = r;
    
    oGC.beginPath();
    oGC.arc(this.x,this.y,this.r,0,360*Math.PI/180.false)
    oGC.closePath();
    oGC.fill();
}

//重绘
Shape.prototype.reDraw = fucntion(point){
    oGC.beginPath();
    oGC.arc(this.x,this.y,this.r,0,360*Math.PI/180.false)
    oGC.closePath();
    oGC.fill();
    
    if(oGC.isPointInPath(point.x,point.y)){
        this.click();
    }
}
```



**—— 人眼一般能接受的动画60帧，即1秒中循环60次**

```setInterval(function(){    //动画 },1000/60) ```

