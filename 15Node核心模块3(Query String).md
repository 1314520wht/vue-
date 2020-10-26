#### Node核心模块(QueryString)
##### 一、QueryString模块简介
querystring从字面上的意思就是查询字符串，一般是对http请求所带的数据进行解析。querystring模块只提供4个方法。

###### 1. querystring.parse：将一个字符串反序列化为一个对象
语法：querystring.parse(str[, sep[, eq[, options]]])
- str需要反序列化的字符串

- sep可选，用于分割str字符串的字符或字符串，默认为"&"

- eq可选，用于划分键和值的字符或字符串，默认为"="

- options可选，是个对象，可设置maxKeys属性，传入一个number类型值，指定解析键值对的最大值，默认1000，如果设置为0，则取消解析的数量限制。{maxKeys:2}

- ```js
  let querystring = require("querystring")
  
  let url = querystring.parse("name=jianan&url=http://www.tanzhouedu.com/")
  
  console.log(url) 
  // [Object: null prototype] {
  //     name: 'jianan',
  //     url: 'http://www.tanzhouedu.com/'
  //   }
  
  
  let url2 = querystring.parse("name*jianan#url*http://www.tanzhouedu.com/#age*5", "#" ,"*", {maxKeys:2})
  
  console.log(url2)
  //   [Object: null prototype] {
  //     name: 'jianan',
  //     url: 'http://www.tanzhouedu.com/'
  //   }
  ```

  
###### 2. querystring.stringify：将对象序列化成一个字符串，跟parse相对
语法：querystring.stringify(obj[, sep[, eq[, options]]])
- obj需要序列化的对象

- sep可选，用于连接键值对的字符或字符串，默认为"&"

- eq可选，用于连接键和值的字符或字符串，默认为"="

- options可选，是个对象，可设置encodeURIComponent属性，值类型为function，可以将一个不安全的url字符串转换成百分比的形式，默认为querystring.escape()

- ```js
  let querystring = require("querystring")
  
  let str = querystring.stringify({name:"jianan",age:18})
  
  console.log(str)  //name=jianan&age=18
  
  let str2 = querystring.stringify({name:"jianan",age:18}, "*", "$")
  
  console.log(str2)  //name$jianan*age$18
  
  ```

  
###### 3. querystring.escape：让传入的字符串进行编码
语法：querystring.escape(str)

```js
let querystring = require("querystring")

let str = querystring.escape("name=jianan")
let str2 = querystring.escape("name=佳楠")

console.log(str,str2)  //name%3Djianan name%3D%E4%BD%B3%E6%A5%A0
```



###### 4. querystring.unescape：将含有%的字符串进行解码
语法：querystring.unescape(str)

```js
let querystring = require("querystring")

let str = querystring.escape("name=jianan")
let str2 = querystring.escape("name=佳楠")
console.log(str,str2)  //name%3Djianan name%3D%E4%BD%B3%E6%A5%A0

let unstr = querystring.unescape(str)
let unstr2 = querystring.unescape(str2)

console.log(unstr, unstr2)  //name=jianan name=佳楠

```

