#### WebSocket
##### 一、简介
WebSocket是一种网络传输协议，允许**服务器端主动向客户端推送数据**，它让客户端和服务器之间的数据交换变得更加简单。

##### 二、与HTTP对比
首先我们来思考一个问题，我们已经有了http协议，为何还需要WebSocket？

HTTP协议是无状态的，服务器只会响应来自客户端的请求，但是它与客户端之间不具备持续连接。它无法轻松实现实时应用。只有发送个请求，他才会响应。

那么在传统的应用当中，如果客户端想要获取最新的信息，我们要怎么做呢？

##### 三、事件
事件|用法
---|---
open|连接建立时触发
message|客户端接收服务端数据时触发
error|通信发生错误时触发
close|连接关闭时触发

##### 四、方法
方法|用法
---|---
Socket.send()|使用连接发送数据
Socket.close()|关闭连接

五。使用

1.新建websocket-ws文件。npm init -y 安装项目依赖文件package.json。

2npm i  -S    ws。安装ws包

补充：mkdir   文件   。新建文件。