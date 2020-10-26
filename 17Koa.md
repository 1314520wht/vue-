#### Koa
基于Node.js平台的下一代web开发框架
##### 一、简介
koa 是由 Express 原班人马打造的，致力于成为一个更小、更富有表现力、更健壮的 Web 框架。使用 koa 编写 web 应用，通过组合不同的 generator，可以免除重复繁琐的回调函数嵌套，并极大地提升错误处理的效率。koa 不在内核方法中绑定任何中间件，它仅仅提供了一个轻量优雅的函数库，使得编写 Web 应用变得得心应手。
##### 二、应用
Koa应用是一个包含一系列中间件generator函数的对象。这些中间件函数基于request请求以一个类似于栈的结构组成并以此执行。Koa的核心设计思路是为中间件层提供高级语法糖封装，以增强其互用性和健壮性，并使得编写中间件变得相当有趣。
##### 三、起步教程
###### 1. 直接安装koa包，引入使用
- 1、使用npm安装koa包
```cmd
npm i koa
```
- 2、创建一个app.js文件，文件内容如下：
```js
const Koa = require('koa')
const app = new Koa()
//每次收到一个http请求，就会调用app.use()注册的async函数，并且传入ctx和next参数
app.use(async (ctx, next) => {
    await next()
    ctx.response.type = 'text/html'
    ctx.response.body = '<h1>Hello, koa2!</h1>'
    //ctx包含了req和res.可以直接通过ctx.response写
})

app.listen(3000)
```
- 3、启动项目，在项目目录打开cmd,执行以下命令
```cmd
node app.js

```
恭喜你，已经成功的开启了Koa的世界大门，来跟着佳楠老师学习第二种入门方法吧。
###### 2. 配置package.json，使用npm安装依赖
- 1、在项目目录下，打开cmd,快速初始化一个项目依赖文件package.json
```cmd
npm init -y
```
- 2、在package.json文件中配置我们项目中所需要的依赖包，并且自定义项目启动命令
```json
{
  "name": "koa-demo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node app.js" // 自定义项目启动命令
  },
  "dependencies": {
    "koa": "2.13.0" //配置项目需要的依赖包
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```
- 3、接下来在项目目录下打开cmd使用npm安装我们的依赖包
```cmd
npm i
```
- 4、在cmd中运行自定义的启动命令，就可以使用我们的Koa了！
```cmd
npm start
```
##### 四、进阶教程
接下来就是我们学习Koa的大菜了，我会以实践的方式带领大家一起学习Koa!

```js
//通过不同的路由。来显示不同的内容
const Koa = require("Koa")

const app = new Koa()

app.use(async (ctx,next)=>{
    //通过不同的请求，来返回不同的信息
    // console.log(ctx.request)
    if(ctx.request.path === "/"){ //判断根路径的内容
        ctx.response.body = "index page"
    }else{
        await next()
    }
})

app.use(async (ctx,next)=>{
    if(ctx.request.path === "/user"){
        ctx.response.body = "user page"
    }else{
        await next()
    }
})


app.listen(3000)

console.log("app started at port 3001")

```

```js
//实现简单登录
//简单登录
const Koa = require("Koa")
const router = require("koa-router")() //引入中间件 需要下载一下
const bodyParser = require("koa-bodyparser") //引入可解析request.body的件  也得下载
const app = new Koa()

app.use(async (ctx,next)=>{
   await next()
})
router.get("/hello/:name", async (ctx,next)=>{ //路由
    let name =JSON.stringify( ctx.params)
    ctx.response.body = `${name}`
})
router.get("/",async (ctx,next)=>{ //路由
    ctx.response.body = `<form action="/login" method="post">
    <p>姓名：<input type="text" name="name"></p>
    <p>密码：<input type="password" name="password"></p>
    <p><input type="submit" value="submit"></p>
</form>`
})
router.post("/login",async (ctx,next)=>{
    let name = ctx.request.body.name,
        password = ctx.request.body.password
    if(name === "admin" && password === "12345"){
        ctx.response.body = `<h1>hello ${name}</h1>`
    }else{
        ctx.response.body = `<h1>login failed</h1>
                            <p><a href="/">again login</p>`
    }    
})
app.use(bodyParser())
app.use(router.routes())
app.listen(3003)
console.log("app started at port 3003")

```

