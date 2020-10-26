#### NPM
##### 一、NPM简介

包管理工具

npm已经在Node.js安装的时候顺带装好了。我们在命令提示符或者终端输入npm -v，可以看到npm的版本号。

##### 二、NPM相关网站以及使用
NPM官网：[https://www.npmjs.com/]
NPM中文网：[https://www.npmjs.cn/]
淘宝NPM镜像：[https://developer.aliyun.com/mirror/NPM?from=tnpm]

通过径像在cmd中下载cnpm。输入：npm install -g cnpm --registry=https://registry.npm.taobao.org

cnpm完事之后还得npm i一下，防止漏包。

cnpm -v查看cnpm版本



##### 三、package.json
我们使用package.json来管理依赖
在cmd中，我们可以使用npm init来初始化一个package.json文件，用回答问题的方式生成一个新的package.json文件。

```cmd
npm init
```
快速创建:
```cmd
npm init -y
```
使用一下命令能安装所有依赖
```cmd
npm install
```
package.json介绍官网：[http://docs.npmjs.com/files/package.json]

下载包。

1.先创建个文件夹。通过在该文件夹下cmd中输入npm init -y下载package.json模板。然后在package.json文件中，在dependencies里面添加需要下载使用的包名。如： "dependencies": {

  "time-stamp": "^2.2.0",

  "jquery": "^3.5.1"

 },这时在cmd中输入npm i进行包的下载。



或者要下载需要用的包名：npm i --save time-stamp(包名) 比如全局下载包time-stamp