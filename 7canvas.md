#### 第7课：Canvas
##### 一、Canvas基础知识
1. canvas是HTML5新增元素，并且它是用js给我们的页面绘制图形的。
2. 其本身是一个画布，并不能绘制图形，而是通过js当作画笔动态渲染的，而画笔的工具为getContext('2d')。
3. 其浏览器支持情况如下：IE9+、Firefox1.5+、Safari2+、Opera9+、Chrome、IOS版Safari以及Android版WebKit。
4. 它是一个标签```<canvas></canvas>```,其标签可以像div一样，设置所有的属性。跟img对比，它没有alt、src，但是其本身拥有width、height。默认宽高为300*150。
5. canvas的绘制符合中国的传统美学，白底黑字，背景为白色，线条为黑色。
6. canvas用法（**注意：设置canvas样式的时候，使用它默认是行内样式进行设置，如果用style进行设置，会导致canvas变形。**）
```html
<canvas id='myCanvas' width='500' height='500' style='border:1px solid #ccc'></canvas>
```
```js
var canvas = document.getElementById('myCanvas').getContext('2d')
canvas.fillStyle = 'red'
canvas.fillRect(50,50,200,200)
canvas.strokeStyle='blue'
canvas.strokkeRect(250,250,200,200)
```
7. canvas起始端点为浏览器内画布位置的坐上角。
##### 二、绘制基本图形
1. **矩形的绘制**
```js
// 方法一、绘制实心矩形，语法：fillRect(x,y,w,h)。x,y,w,h表示绘制起点所在位置的x,y坐标，以及绘制图形的宽高。
canvas.fillRect(0,0,100,100)
canvas.fillStyle = 'yellow' // 可使用fliiStyle改变颜色

// 方法二、绘制空心矩形，语法：strokeRect(x,y,w,h)
canvas.strokeRect(0,0,100,100)

// 方法三、路径绘制(closePath可以省略，带上比较好)
canvas.beginPath() // 开始绘制
canvas.rect(100,100,200,200)
canvas.closePath() //closePath要在前面提前结束
canvas.fillStyle = 'yellow' //设置实心颜色
canvas.fill() // 调用实心方法
// canvas.strokeStyle = 'red' // 设置空心颜色
// canvas.stroke() // 调用空心方法
```
2. **三角形的绘制**
```js
// 路径法绘制三角形(fill可以不用closePath，stroke必须要用closePath)
canvas.beginPath()
canvas.moveTo(50,50) // 定义起始点
canvas.lineTo(50,300) // 定义连接点
canvas.lineTo(300,50)
canvas.closePath()
// canvas.lineTo(50,50)//如果不适用closePath而只用这行代码绘制三角形，那么我们看到的三角形的连接方式是不正确的
canvas.lineWidth = 5 //线宽
canvas.stroke() //空心必须要关闭 closePath
// canvas.fill() // 实心方法可以省略closePath
```
3. **圆形的绘制**

   

   
```js
// 绘制圆形用路径法，六个参数(x,y,r,startAngle,endAngle,false)圆心x坐标,圆心y坐标,半径，开始弧度，终止弧度，是否逆时针(课省略不写，默认false)
canvas.beginPath()
canvas.arc(100,100,100,0,Math.PI*2,false) // Math.PI*2 = 2∏
canvas.closePath()
canvas.stroke()
```
4. **绘制端点和线条连接**
  + 4.1端点绘制
```js
// 通过lineCap属性可以控制线条末端的形状是平头、圆头还是方头(butt、round、square)
canvas.lineWidth = 20
// butt 平头
canvas.beginPath()
canvas.moveTo(50,50)
canvas.lineTo(50,200)
canvas.stroke()
// round  圆头
canvas.beginPath()
canvas.moveTo(100,50)
canvas.lineTo(100,200)
canvas.lineCap = 'round'
canvas.stroke()
// square 方头
canvas.beginPath()
canvas.moveTo(150,50)
canvas.lineTo(150,200)
canvas.lineCap = 'square'
canvas.stroke()

// 我们建立两条辅助线，帮助我们观察
canvas.lineWidth = 1
canvas.beginPath()
canvas.moveTo(0,50)
canvas.lineTo(200,50)
canvas.stroke()

canvas.beginPath()
canvas.moveTo(0,200)
canvas.lineTo(200,200)
canvas.stroke()
```
  + 4.2线条连接
```js
// 通过lineJoin属性可以控制线条相交的方式是圆交、斜交还是斜接(round、bevel、miter)
canvas.beginPath()
canvas.moveTo(50,50)
canvas.lineTo(50,300)
canvas.lineTo(300,50)
canvas.closePath()// 是最后一个端点与首端点的连接，也称闭合路径
canvas.lineWidth = 10
// canvas.lineJoin = 'round'
// canvas.lineJoin = 'bevel'
canvas.lineJoin = 'miter'
canvas.stroke()
```
5. **弧线、渐变与文本**
  + 5.1弧线
```js
// arcTo(x1,y1,x2,y2,radius)从上一点开始绘制弧线，到(x2,y2)为止，并且以给定的半径radius穿过(x1,y1)
canvas.beginPath()
canvas.moveTo(20,20) // 创建开始点
canvas.lineTo(100,20)// 创建水平线
canvas.arcTo(150,20,150,70,50) // 创建弧
canvas.lineTo(150,120) // 创建垂直线
canvas.stroke() // 进行绘制
```
```js
// 二次贝塞尔曲线quadraticCurveTo(cx,cy,x,y)从上一点开始绘制一条二次曲线，到(x,y)为止，并且以(cx,xy)作为控制点
canvas.beginPath()
canvas.moveTo(20,20)
canvas.lineTo(100,20)
canvas.quadraticCurveTo(200,50,200,100)
canvas.stroke()
```
```js
// 三次贝塞尔曲线bezierCurveTo(cp1x,cp1y,cp2x,cp2y,x,y)两个控制点位置坐标，加上终止位置坐标
canvas.beginPath()
canvas.moveTo(75,40)// 心形中间的上点
canvas.bezierCurveTo(75,37,70,25,50,25)
canvas.bezierCurveTo(20,25,20,62.5,20,62.5)
canvas.bezierCurveTo(20,80,40,102,75,120) // 心形最下端的点
canvas.bezierCurveTo(110,102,130,80,130,62.5)
canvas.bezierCurveTo(130,62.5,130,25,100,25)
canvas.bezierCurveTo(85,25,75,37,75,40)
canvas.fillStyle = 'red'
canvas.fill()
```
  + 5.2渐变
    渐变分为两种，线性渐变和径向渐变
```js
// 线性渐变createLinearGradient(startX,startY,endX,endY)起点的x,y坐标，终点的x,y坐标
// addColorStop两参数:百分比，颜色
var gradient = canvas.createLinearGradient(0,0,500,500)
gradient.addColorStop(0,'red')
gradient.addColorStop(.2,'orange')
gradient.addColorStop(.3,'yellow')
gradient.addColorStop(.5,'green')
gradient.addColorStop(.6,'blue')
gradient.addColorStop(.8,'skyblue')
gradient.addColorStop(1,'purple')
canvas.beginPath()
canvas.arc(250,250,250,0,Math.PI*2)
canvas.closePath()
canvas.fillStyle = gradient
canvas.fill()
```
```js
// 径向渐变createRadialGradient(startX,startY,startrR,endX,endY,endR)起点圆的圆心半径和终点圆的圆心半径
// addColorStop两参数:百分比，颜色
var gradient = canvas.createRadialGradient(250,250,10,250,250,250)
gradient.addColorStop(0,'red')
gradient.addColorStop(.2,'orange')
gradient.addColorStop(.3,'yellow')
gradient.addColorStop(.5,'green')
gradient.addColorStop(.6,'blue')
gradient.addColorStop(.8,'skyblue')
gradient.addColorStop(1,'purple')
canvas.beginPath()
canvas.arc(250,250,250,0,Math.PI*2)
canvas.closePath()
canvas.fillStyle = gradient
canvas.fill()
```
  + 5.2文本
    绘制文本主要有两个方法：fillText()和strokeText()
    这两个方法都可以接收四个参数:要绘制的文本字符串、x坐标、y坐标、可选的最大像素宽度。
    这两个方法都以font、textAlign、textBaseline这三个属性为基础。
```js
canvas.font = 'normal 30px Yahei'
canvas.textAlign = 'start'
canvas.textBaseline = 'top'
canvas.fillText('9期框架-佳楠',50,50)
// canvas.strokeText('9期框架-佳楠',50,50)
```
6. **阴影、图像**
  + 6.1阴影
    绘制阴影，要注意这四个参数：
    1.shadowColor：表示颜色
    2.shadowOffsetX：表示X方向的偏移量
    3.shadowOffsetY：表示Y方向的偏移量
    4.shadowBlur：表示模糊度
```js
canvas.shadowColor = 'rgba(255,0,0,.5)'
canvas.shadowOffsetX = 10
canvas.shadowOffsetY = 10
canvas.shadowBlur = 5
canvas.fillRect(50,50,200,200)
```
  + 6.1图像
    drawImage()绘制图像主要有以下三种方法：
```js
// 1.基础款-三参数法则：分别对应img,x,y
var img = new Image()
img.src = './cat.jpg'
img.onload = function() {
canvas.drawImage(img,0,0)
}
// 2.尊享款-五参数法则：分别对应img,x,y,w,h
var img = new Image()
img.src = './cat.jpg'
img.onload = function() {
canvas.drawImage(img,0,0,400,300)
}
// 3.至尊款-九参数法则：img,x1,y1,w1,h1,x,y,w,h(源图像纵横坐标图片宽高，目标图像纵横坐标图片宽高)
var img = new Image()
img.src = './cat.jpg'
img.onload = function() {
canvas.drawImage(img,0,0,533,300,0,0,300,200)
}
```
7. 综合案例：绘制手表

   ```html
   <!DOCTYPE html>
   <html lang="zh">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>第九期框架-佳楠</title>
   </head>
   <body>
       <canvas id='canvas' width="500" height="500" style="margin:50px 30%;"></canvas>
   
       <script>
           var cas = document.getElementById('canvas').getContext('2d')
           setInterval(function() {
               drawClock()
           },1000)
           function drawClock() {
               // 清空画布clearRect，清空矩形内的指定像素
               // x,y,w,h
               cas.clearRect(0,0,500,500)
   
               // 1.绘制表盘
               cas.beginPath()
               cas.arc(250,250,200,0,Math.PI*2)
               cas.lineWidth = 8
               cas.strokeStyle = 'black'
               cas.stroke()
   
               // 2.绘制刻度
               // 2.1.1绘制时针刻度
               for(let i = 0; i < 12; i++) {
                   cas.save() // 保存当前的状态，始终让画布位于第一次的0度状态
                   cas.translate(250,250) // 移动到旋转位置的中心点
                   cas.rotate(i*Math.PI/6)
                   cas.beginPath()
                   cas.moveTo(0, -200)
                   cas.lineTo(0, -175)
                   cas.lineWidth = 5
                   cas.stroke()
                   cas.restore() //调取当前状态的保存值，在每次调用时都能获取这个保存的值
               }
               // 2.1.2绘制分针刻度
               for(let i = 0; i < 60; i++) {
                   cas.save() // 保存当前的状态，始终让画布位于第一次的0度状态
                   cas.translate(250,250) // 移动到旋转位置的中心点
                   cas.rotate(i*Math.PI/30)
                   cas.beginPath()
                   cas.moveTo(0, -200)
                   cas.lineTo(0, -185)
                   cas.lineWidth = 3
                   cas.stroke()
                   cas.restore() //调取当前状态的保存值，在每次调用时都能获取这个保存的值
               }
               // 3.获取世界时间（当前）
               var now = new Date(),
                   hour24 = now.getHours(),
                   minute = now.getMinutes(),
                   second = now.getSeconds()
               var hour = hour24 + minute/60,
                   hour = hour24 > 12 ? hour-12 : hour
                   console.log(hour)
   
   
               // 4.绘制时针分针
               // 4.1绘制时针
               cas.save()
               cas.translate(250,250)
               cas.rotate(hour*Math.PI/6)
               cas.beginPath()
               cas.moveTo(0, 10)
               cas.lineTo(0, -110)
               cas.lineWidth = 5
               cas.lineCap = 'round'
               cas.stroke()
               cas.restore()
   
               // 4.2绘制分针
               cas.save()
               cas.translate(250,250)
               cas.rotate(minute*Math.PI/30)
               cas.beginPath()
               cas.moveTo(0, 15)
               cas.lineTo(0, -130)
               cas.lineWidth = 5
               cas.lineCap = 'round'
               cas.stroke()
               cas.restore()
   
               // 4.3绘制秒针
               cas.save()
               cas.translate(250,250)
               cas.rotate(second*Math.PI/30)
               cas.beginPath()
               cas.moveTo(0, 20)
               cas.lineTo(0, -150)
               cas.lineWidth = 1
               cas.lineCap = 'round'
               cas.strokeStyle = 'blue'
               cas.stroke()
               cas.restore()
               
               // 4.4绘制表盘的圆点
               cas.beginPath()
               cas.arc(250,250,5,0,Math.PI*2)
               cas.lineWidth = 2
               cas.strokeStyle = 'blue'
               cas.stroke()
               cas.fillStyle = 'white'
               cas.fill()
   
               // 4.5写文字
               cas.fillStyle = 'black'
               cas.font = '18px Arial'
               cas.textAlign = 'center'  //文本居中
               cas.fillText('Time is money',250,150)
           }
   
       </script>
   
   </body>
   </html>
   ```

8. 绘制太极图：

   ```html
   <!DOCTYPE html>
   <html lang="zh">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>第九期框架-佳楠</title>
       <style>
           * {
               margin: 0;
               padding: 0;
           }
           body {
               background: #ccc;
           }
           #can {
               margin: 30px 400px;
               animation: trun 6s linear infinite;
           }
           @keyframes trun {
               0%{transform: rotate(0deg)}
               25%{transform: rotate(90deg)}
               50%{transform: rotate(180deg)}
               75%{transform: rotate(270deg)}
               100%{transform: rotate(360deg)}
           }
       </style>
   </head>
   <body>
       <canvas id="can" width="500px" height="500px"></canvas>
       <script>
           var cas = document.getElementById('can').getContext('2d')
   
           // cas.beginPath()
           // cas.arc(250,250,250,Math.PI*1/2,Math.PI*3/2)
           // cas.fill()
   
           // cas.beginPath()
           // cas.arc(250,250,250,Math.PI*3/2,Math.PI*1/2)
           // cas.fillStyle = '#fff'
           // cas.fill()
   
           // cas.beginPath()
           // cas.arc(250,125,125,0,Math.PI*2)
           // cas.fillStyle = '#000'
           // cas.fill()
   
           // cas.beginPath()
           // cas.arc(250,375,125,0,Math.PI*2)
           // cas.fillStyle = '#fff'
           // cas.fill()
   
           // cas.beginPath()
           // cas.arc(250,125,40,0,Math.PI*2)
           // cas.fillStyle = '#fff'
           // cas.fill()
   
           // cas.beginPath()
           // cas.arc(250,375,40,0,Math.PI*2)
           // cas.fillStyle = '#000'
           // cas.fill()
   
           function arc(cas,x,y,r,start,end,color) {
               cas.beginPath()
               cas.arc(x,y,r,start,end)
               cas.fillStyle = color
               cas.fill()    
           }
   
           arc(cas,250,250,250,Math.PI*1/2,Math.PI*3/2,'#000')
           arc(cas,250,250,250,Math.PI*3/2,Math.PI*1/2,'#fff')
           arc(cas,250,125,125,0,Math.PI*2,'#000')
           arc(cas,250,375,125,0,Math.PI*2,'#fff')
           arc(cas,250,125,40,0,Math.PI*2,'#fff')
           arc(cas,250,375,40,0,Math.PI*2,'#000')
   
       </script>
   </body>
   </html>
   ```

   

9. 综合;x,y代表起点  w:宽  h：高

   ​     只要有线，都可以设置lineWidth 线宽

     （1）矩形：filerRect(x,y,w,h)     stokeRect(x,y,w,h)    路径绘制：rect(x,y,w,h)

     （2）三角形：moveTo(x,y)   lineTo(x,y)   lineTo(x,y)    实心不用closePath()   空心必须closePath() .完事之后得调用实心方法或空心方法

    （3）圆 ：arc(x,y,r,0,Math.PI*2)  圆心坐标，半径，开始弧度，结束弧度

    （4）端点绘制：lineCap = "默认平头/round圆头/square方头"

    （5）线条连接：fill用于线条连接没效果。用stroke()

   ​           lineJoin = "round圆交/bevel斜交/miter斜接"

    （6）基础弧线：只需要一个lineTo即可，作为起点。arcTo(x1,y1,x2,y2,r) 以x2,y2作为终点，r为半径，在x1,y1处进行拉伸

     （7）二次贝塞尔曲线：quadraticCurveTo(200,20,200,100)//200,100为终点，200,50为拉伸点

    （8）三次贝塞尔曲线：

   

   

   

     
