#### Node核心模块(URL、Path)
##### 一、URL模块
###### 1.1 parse:拿到url地址每一部分
Node.js中的url模块提供了一些实用函数，用于URL处理与解析。
语法：url.parse(urlStr,parseQueryString,slashesDenoteHost)
- urlStr是输入一个url字符串，返回一个对象

- parseQueryString(默认为false)

- slashesDenoteHost(默认为false)

- ```js
  //url模块
  let url = require("url")
  
  let path = "http://www.baidu.com:8000/p/a/t/h/?query=string#hash"
  
  let urlInfo = url.parse(path, true)
  
  console.log(urlInfo)  
  // Url {
      
  //   protocol: 'http:',
  //   slashes: true,
  //   auth: null,
  //   host: 'www.baidu.com:8000',
  //   port: '8000',
  //   hostname: 'www.baidu.com',
  //   hash: '#hash',
  //   search: '?query=string',
  //   query: [Object: null prototype] { query: 'string' },
  //   pathname: '/p/a/t/h/',
  //   path: '/p/a/t/h/?query=string',
  //   href: 'http://www.baidu.com:8000/p/a/t/h/?query=string#hash'
  // }
  
  ```

  ```js
  //html数据提交模块
  <!DOCTYPE html>
  <html lang="zh">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>第九期框架-佳楠</title>
  </head>
  <body>
      <form action="http://127.0.0.1:3000">
          name: <input type="text" name="user">
          age:<input type="text" name="age">
          <input type="submit" value="submit">
      </form>
  </body>
  </html>
  ```

  ```js
  //获取提交的信息。
  let http = require("http")
  let url = require("url")
  
  let server = http.createServer(function(req,res) {
      let path = req.url
      // console.log(path)
      if (path === "/favicon.ico") {
          return
      }
      let query = url.parse(req.url, true).query
      console.log(query)  //[Object: null prototype] { user: 'li1', age: '90' }
      let user = query.user
      let age = query.age
      // console.log(query)
      console.log("用户名:"+ user,"年龄："+ age)
      res.end()
  })
  
  server.listen(3000)
  ```

  

##### 二、Path模块
path模块提供许多使用的，可被用来处理与转换路径方法与属性。
###### 2.1 basename方法:获取路径中的文件名
语法：path.basename(path[,ext])
- path文件的完整路径

- ext为可选参数

- ```js
  let path = require("path")
  
  let fileName = path.basename("./01base.txt")
  console.log(fileName)  //01base.txt
  
  let fileName2 = path.basename("./01base.txt", ".txt")
  console.log(fileName2)   //01base  
  
  
  ```

  
###### 2.2 dirname方法:获取路径中的目录名
语法：path.dirname(path)
- path可以是文件、目录路径。参数是目录路径时，返回目录的上层目录，参数为文件路径时，返回文件所在目录。

- ```js
  let path = require("path")
  
  let dirname = path.dirname("./02dir/test.js")
  
  console.log(dirname)   //./02dir
  
  ```

  
###### 2.3 extname方法:获取路径中的扩展名
语法：path.extname(path)
- path返回文件的扩展名，若文件没有指定扩展名，则返回空字符串。

- ```js
  let path = require("path")
  
  let extname = path.extname("./03ext_test.txt")
  
  console.log(extname)  //.txt
  
  ```

  
###### 2.4 join方法:将多个参数值字符串结合成一个路径字符串
语法：path.join([path1], [path2], [...])
此方法中，讲参数值结合生成一个路径
__dirname变量值代表程序运行的根目录

```js
let path = require("path")

let joinPath = path.join("/test", "/a", "/b")
console.log(joinPath) //\test\a\b

let joinPath2 = path.join(__dirname, "/demo", "/src") //后面两个是添加的路径
console.log(joinPath2) //C:\Users\dell\Desktop\web前端\vue\vue课堂代码\15Node核心模块\Node核心模块\05-path\demo\src
```

