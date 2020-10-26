#### CSS预处理
##### 一、什么是CSS预处理器？
CSS预处理器定义了一种新的语言，其基本思想是用一种专门的编程语言，为CSS增加一些编程的特性，将CSS作为目标生成文件，然后开发者就只要使用这种语言进行编码工作。

**优点：使用CSS预处理器语言，可以让我们的CSS代码更加简洁、适应性更强、可读性更佳、更易于维护等诸多好处。**

目前比较优秀的CSS预处理器语言有Less、Sass(Scss)、Stylus。这里我们学习Less和Sass。

Less和Sass包含了一套自定义的语法及一个解释器，用户根据这些语言定义自己的样式规则，这些规则会通过解析器，编译生成对应的CSS文件，只有在被编译后才能够被浏览器识别使用。编译Less、Sass的方法有很多，比如通过命令行编译、编辑器编译、在线编译、第三方工具等。这里我们来介绍一款比较古老的工具--(Koala)考拉。

##### 三、Less语法学习
- 1.注释
less有两种注释风格。
标准的CSS注释/* code */，会保留到编译后的文件。
单行注释 // code，只保留在less源文件中，编译后被省略。
- 2.import导入样式
引入.css文件  同于css的import命令
```css
@import 'reset.css'
```
编译后：
```css
@import reset.css
```
.css后缀名不能省略  引入.less文件可以省略扩展名
```css
@import 'reset'
```
编译后：导入的文件会与当前的文件内容合并
- 3.变量

Less允许开发者自定义变量，变量可以在全局样式中使用，变量使得样式修改起来更加简单。
(1)变量以@开头，变量名与变量值之间用**冒号**分割
```less
@width: '20px'
#header {
    width: @width;
}
```
(2)变量嵌套在字符串时，必须写在@{}之中
```less
@side: left;
.round {
    border-@{side}-radius: 5px;
}
```
- 4.嵌套
(1)选择器嵌套
```less
p {
    font-size: 20px;
    a {
        text-decoration: none;
        &:hover { border-width: 1px }
        // 在嵌套在代码块内，可以使用&引用父元素。
    }
}
```
- 5.混合
混合可以将一个定义好的class A 轻松的引入到另一个 class B中，从而实现 class B 继承 classA中所有的属性。
5.1混合之继承类
```less
.border {
    border: 1px dotted yellow;
}
.main {
    color: #fff;
    .border;
}
```
5.2混合参数没有设置默认值，此时调用必须传入参数
```less
// 1.定义
.top (@top) {
    top: @top;
}
// 2.调用
#main {
    .top(30px);
}
```
5.3混合参数并且设置了默认值，调用时可以不用传入参数。
```less
// 1.定义
.top (@top: 25px) {
    top: @top;
}
// 2.调用时不传参数，则使用默认值25px
#main {
    .top;
}
// 3.若调用时传入参数，则使用传入的值。
#cont {
    .top(50px);
}
```
5.4混合参数中使用@arguments来引用所有传入的变量
```less
// 1.定义
.box-shadow (@x: 0,@y :0,@blur: 1px, @color: #000) {
    box-shadow: @arguments;
    -moz-box-shadow: @arguments;
    -webkit-box-shadow: @arguments;
}
// 2.调用
div {
    .box-shadow (10px, 5px)
}
```
- 6.继承：extend伪类来实现样式的继承使用
混合已经可以实现很简单的继承了，为什么还要使用extend来实现继承呢。这是因为混合实现继承的原理是把代码copy一份过来。编译后的css代码中会有大量的冗余代码，而extend则能很好的避免这一点问题。
```less
.public {
    background: yellow;
}
li {
    &:extend(.public);
    list-style: none;
}
// 等同于
li:extend(.public) {
    list-style: none;
}
```
上面代码编译后的css结果是
```css
.public, li {
    background: yellow;
}
li {
    list-style: none;
}
```
- 7.运算
任何的数字、颜色或者变量都可以参与运算，运算应该被包裹在括号中,以()进行优先级计算。
```less
.width {
    width: (200px - 30)*2;
}
// 编译后
.width {
    width: 340px;
}
```
##### 四、Sass语法学习
- 1.注释:Sass跟Less的注释语法一样

  ### 五。less使用。

  1.新建文件夹，在该目录里打开cmd.输入npm i -g less 下载less

  2.创建一个index.less文件。

  3.接着cmd中输入 lessc index.less  index.css。就可以生成css文件，只要在less中写代码，就可以在css中同步显示。

  ### 六.echarts图表使用。

  网上有它教程。详细。

  首先新建文件夹，然后

  1.npm install echarts --save

  2.html文件中src引入https://unpkg.com/echarts@4.8.0/index.js。

  3.在绘图前我们需要为 ECharts 准备一个具备高宽的 DOM 容器。

  ```html
  <body>
      <!-- 为 ECharts 准备一个具备大小（宽高）的 DOM -->
      <div id="main" style="width: 600px;height:400px;"></div>
  </body>
  ```

  4.然后就可以通过 [echarts.init](https://echarts.apache.org/zh/api.html#echarts.init) 方法初始化一个 echarts 实例并通过 [setOption](https://echarts.apache.org/zh/api.html#echartsInstance.setOption) 方法生成一个简单的柱状图，下面是完整代码。

  5.完整代码案例

  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <meta charset="utf-8">
      <title>ECharts</title>
      <!-- 引入 echarts.js -->
      <script src="echarts.min.js"></script>
  </head>
  <body>
      <!-- 为ECharts准备一个具备大小（宽高）的Dom -->
      <div id="main" style="width: 600px;height:400px;"></div>
      <script type="text/javascript">
          // 基于准备好的dom，初始化echarts实例
          var myChart = echarts.init(document.getElementById('main'));
  
          // 指定图表的配置项和数据
          var option = {
              title: {
                  text: 'ECharts 入门示例'
              },
              tooltip: {},
              legend: {
                  data:['销量']
              },
              xAxis: {
                  data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
              },
              yAxis: {},
              series: [{
                  name: '销量',
                  type: 'bar',
                  data: [5, 20, 36, 10, 10, 20]
              }]
          };
  
          // 使用刚指定的配置项和数据显示图表。
          myChart.setOption(option);
      </script>
  </body>
  </html>
  ```

  这时，一个简单的图表就生成了。

### scss  在脚手架中npm 的使用。

1.npm i  --save-dev  sass-loader  node-sass

2.在build文件夹。webpack.base.conf.js中配置一下

```
在rules:[
   {
     test:/\.scss$/,
     loader:['style','css','sass']
   }
]
添加一下这个对象
```

3.重新泡一下项目 npm run dev 。因为改了配置文件。

4.在style里面可以使用lang="scss"

```
<style lang='scss'>
<style>
```

5.如果有报错看不懂，则复制一下错误，在有到词典翻译一下，就知道了。在百度一下。

6.版本可能会有点高sass-loader:"^7.3.1''需要改成7.3.1。再npm i 一下，重新装。

7.