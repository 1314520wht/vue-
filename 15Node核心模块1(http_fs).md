[TOC]

#### Node核心模块(HTTP、fs)
在Node中，想要使用哪一个模块，我们就直接require那个模块。
##### 一、HTTP模块(node.js可以使用http模块来搭建一个服务器)
http模块不是基于特定语言的，是一个通用的应用层协议。
1. METHODS：是一个数组，里面存储着所有支持的请求方法。

2. STATUS_CODES：是一个http模块基本状态类对象，属性名是状态码，属性值则是该状态码的简短解释。

3. http请求：get ,post , put(修改),patch(批量修改)，delete(删除)。

   ```js
   1.引入http模块
   2.使用http创建服务器(http.createServer())
   3.监听端口
   let http = require("http")
   
   // console.log(http)
   let server = http.createServer(function(req,res) {
       
       res.writeHead(200, {"Content-type":"text/html;charset=UTF-8"})
       // res.writeHead(404, {"Content-type":"text/html;charset=UTF-8"})
       // res.write("页面资源找不到")
       res.write("<h1>Hello Http</h1>") // 在end之前这里可以写很多个
       res.end("hello") // 必须要有end方法结束，否则请求一直在等待
   })
   
   // 监听服务器端口
   // server.listen(4000)
   server.listen(3000, 'localhost')
   // server.listen({
   //     port: 5000,
   //     host: "localhost"
   // })
   ```

   
##### 二、fs模块
主要用来处理文件和目录的读写、复制、删除、重命名等操作。fs模块中的所有方法都有异步和同步版本。
**同步方法执行完并返回结果后，才能执行后续的代码。而异步方法采用回调函数接收返回结果，可以立即执行后续代码。**
###### 1、读取文件readFile

还可以读取页面。

语法：fs.readFile(filename, [options], callback)三个参数
- filename是文件名;

- [options]是个可选参数，为一个对象，用于指定文件编码(encodeing)及操作方式(flag);

- callback是回调函数，传递异常err和文件内容data的2个参数。

  ```js
  let fs = require("fs")
  
  fs.readFile("./test.txt","UTF-8", function(err, data) {
      if(err) {
          throw err // 捕捉错误，抛出异常
      }
      // console.log(data.toString())
      console.log(data)
  })
  
  ```

  ```js
  功能：把一个页面，传到服务器，通过判断路径是否是某个确定的路径。需要http开服务器，readfile读取页面。当路由为jianan时，显示页面，否则404.
  req.url 请求的路径
  //判断路径是否是jianan,是则读取页面，如果不是就返回404.
  // localhost:3000/jianan.html
  let http = require("http")
  let fs = require("fs")
  
  let server = http.createServer(function(req,res) {
      let url = req.url  //请求的地址
      res.writeHead(200, {"Content-type":"text/html;charset=UTF-8"})
      if (url === "/jianan") {
          fs.readFile("./test.html",function(err, data) {
              if(err) {
                  throw err
              }
              res.end(data)
          })
      } else {
          res.end("你要访问的页面不存在！")
      }
  })
  
  ```

server.listen(3000, 'localhost')
  ```
  
  
###### 2、写入文件writeFile
语法：fs.writeFile(filename, data, [options], callback)四个参数
- filename是文件名

- data是要写入文件的数据

- options是个可选参数,为一个对象，包含编码格式(encodeing),模式(mode)以及操作方式(flag)

- callback是回调函数

  ```js
  //1.直接写入内容。
  let fs = require("fs")
  
  let data = "Hello ffffff"
  
  fs.writeFile("./null.txt", data, function(err) {
      if (err) {
          throw err
      }
      console.log("写入文件成功！")
  })
  ```

  ```js
  //2.在文件中，追加内容
  let fs = require("fs")
  
  let data = "这是追加的内容"
  
  fs.writeFile("./test2.txt", data, {"flag": "a"}, function(err) {
      if (err) {
          throw err
      }
      console.log("追加内容成功！")
  })
  ```

  

flag的值|作用
---|---
r|读取文件，文件不存在时报错
r+|读取并写入文件，文件不存在时报错
rs|以同步的方式读取文件，文件不存在时报错
rs+|以同步方式读取并写入文件，文件不存在时报错
w|写入文件，文件不存在则创建，存在则清空
wx|和w一样，但是文件存在时会报错
w+|读取并写入文件，文件不存在则创建，存在则清空
wx+|和w+一样，但是文件存在时会报错
a|以追加方式写入文件，文件不存在则创建
ax|和a一样，但是文件存在时会报错
a+|读取并追加写入文件，文件不存在则创建
a+x|和a+一样，但是文件存在时会报错

-----
**使用fs.read()和fs.write()读写文件需要使用fs.open打开文件和fs.close关闭文件。**
###### 3、读取文件read
语法：fs.read(fd, buffer, offset, length, position, callback)六个参数
- fd是文件描述符，必须接收fs.open()方法中的回调函数返回的第二个参数

- buffer是存放读取到的数据的Buffer对象

- offset指定向buffer中存放数据的起始位置

- length指定读取文件中数据的字节数

- position 指定在buffer文件中从起始位置开始不读取几个字节数

- callback回调函数，参数有err(用于抛出异常)
  ，bytesRead(从文件中读取内容符实际字节数)，buffer(被读取的缓存区对象)

  ```js
  let fs = require("fs")
  
  fs.open("./testRead.txt", "r", function(err, fd) {
      if (err) {
          throw err
      }
      let buffer = new Buffer.alloc(255)
      fs.read(fd, buffer, 0, 10, 0, function (err, bytesRead, buffer) {
          if (err) {
              throw err
          }
          console.log(bytesRead, buffer.slice(0, bytesRead).toString())
          fs.close(fd, function(err) {
              if (err) {
                  throw err
              }
              console.log("关闭成功")
          })
      })
  })
  
  ```

  
###### 4、写入文件wrtie
语法：fs.wrtie(fd, buffer, offset, length, position, callback)六个参数。参数和fs.read()相同，buffer是需要写入文件的内容。

```js
let fs = require("fs")

fs.open("./01w_test.txt","w",function(err, fd) {
    if (err) {
        throw err
    }
    fs.write(fd, "hello 这是要写入文件的内容", 0, "utf-8", function(err, w, s ) {
        if (err) {
            throw err
        }
        console.log( w, s)// 写入的字节数和写入的内容
    })
})

```



###### 5、创建目录
语法：fs.mkdir(path, [mode], callback)
- path是需要创建的目录

- [mode]是目录的权限(默认值是0777)可不写

- callback是回调函数

  ```js
  let fs = require("fs")
  
  fs.mkdir("./02dir_test", function(err){
      if (err) {
          throw err
      }
      console.log("创建目录成功！")
  })
  
  ```

  

###### 6、删除目录
语法：fs.rmdir(path, callback),只能删除空目录

```js
let fs = require("fs")

fs.rmdir("./03dir_test", function(err) {
    if (err) {
        throw err
    }
    console.log("删除目录成功")
})

```



###### 7、读取目录
语法：fs.readdir(path, callback)

```js
let fs = require("fs")

fs.readdir("./04dir_test", function(err, files) {
    if (err) {
        throw err
    }
    console.log(files)
    fs.stat("./04dir_test/1.js", function(err, stats) {
        if (err) {
            throw err
        }
        // stats.有isDirectory()和isFile()两个方法，用来判断是不是目录/文件
        if (stats.isFile()) {
            console.log("我是文件")
        } else {
            console.log("我不是文件")
        }
    })
})

```



###### 8、查看文件与目录信息
语法：fs.stat(path, callback)
- path要查看目录/文件的完整路径

- callback操作完成回调函数，err(错误对象),stats(fs.stat一个对象实例，提供如:isFile, isDirectory等方法)

- ```js
  let fs = require("fs")
  
  fs.readdir("./04dir_test", function(err, files) {
      if (err) {
          throw err
      }
      console.log(files)
      fs.stat("./04dir_test/1.js", function(err, stats) {
          if (err) {
              throw err
          }
          // stats.有isDirectory()和isFile()两个方法，用来判断是不是目录/文件
          if (stats.isFile()) {
              console.log("我是文件")
          } else {
              console.log("我不是文件")
          }
      })
  })
  
  ```

  
###### 9、查看文件与目录是否存在 （以经弃用）
语法：fs.exists(path, callback)
- path查看目录文件的完整路径
- callback操作完成的回调函数，exists true存在，false不存在
