#### Express
##### 一、简介
Express 是一种保持最低程度规模的灵活Node.js Web应用程序框架，为Web和移动应用程序提供一组强大的功能。Express框架是后台的Node框架。
##### 二、性能
Express 提供精简的基本Web应用程序功能，而不会隐藏您了解和青睐的Node.js功能。Express不对Node.js已有的特性进行二次抽象，我们只是在它之上扩展了Web应用所需的功能。丰富的HTTP工具以及来自Content框架的中间件随取随用，创建强健、友好的API变得快速又简单。
##### 三、起步教程
###### 1. 配置package.json，使用npm安装依赖
- 1、新建一个项目目录，在项目目录下，打开cmd,快速初始化一个项目依赖文件package.json
```cmd
npm init -y
```
- 2、在项目目录中打开cmd安装Express
```cmd
npm install express --save
```
用--save安装的模块会添加到package.json文件中的dependencies里，以后运行项目目录中的npm i将自动安装依赖项列表中的模块。
- 3、在项目目录下创建app.js文件，文件中写入以下代码
```js
let express = require('express')
let app = express()

app.get('/', function (req, res) { //直接用router,就不用引入了
  res.send('Hello World!')
  //res.status(404).send("页面找不到了") 还可以自定义设置转态码
})

app.listen(3000, function () {
  console.log('Example app listening on port 3000!')
})
```
- 4、在项目目录中打开cmd，运行以下命令
```cmd
node app.js
```
此时在浏览器中访问http://localhost:3000就能看到服务器成功开启。而访问其他的路径则会以404 Not Found响应。
###### 2. 使用Express应用程序生成器
- 1、首先打开cmd安装一下express
- 在根目录下npm
```cmd
npm i express-generator -g
```
- 2、使用以下命令创建我们的第一个Express应用程序
```cmd
express --view=pug Express-demo
```
- 3、根据提示依次执行以下命令，进入项目目录中并且安装依赖
```cmd
cd Express-demo
```
```cmd
npm install
```
- 4、启动程序
在MasOS或Linux上，采用以下命令运行此应用程序
```cmd
$ DEBUG=Expree-demo:* npm start
```
在Windows上，使用以下命令：
```cmd
set DEBUG=Expree-demo:* & npm start | npm start
```
然后在浏览器中输入 http://localhost:3000/ 访问此应用程序。

如果要是想自己再加条路由。则。在router目录下，1.创建如login.js.接着在2.views目录下创建login.pug 。3. 在app.js里引入login.  4.  最后在app.js中app.use("/login",loginRouter)  这一步也可以直接app.use("login",require("./routers/login"))

##### 四、进阶教程
以实例循序渐进进行学习