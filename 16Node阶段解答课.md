#### Node阶段解答课
##### 一、常用NPM命令扩展
```cmd
// 安装淘宝镜像
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
```cmd
npm init | npm init -y创建项目依赖文件package.json
npm i 下载依赖包里的文件
```
```package.json
"dependencies": {
    "gulp": "^3.9.1"
}
// package.json中安装的依赖包前面的符号代表的意思
// ~匹配最近的小版本依赖包，比如~1.1.2会匹配所有1.1.x版本，但不包括1.2.0
// ^匹配最新的大版本依赖包，比如^3.4.5会匹配所有3.x.x的包，包括3.5.0，但不包括4.0.0
// *安装最新版本的依赖包
```
- 1.安装模块
```cmd
npm install   // 项目中存在package.json文件并且写入了依赖配置时，会下载所有的依赖包
```
```cmd
npm install 包名   // 本地安装依赖包
```
```cmd
npm install 包名 -g   // 全局安装依赖包
```
```cmd
npm install 包名@版本号   // 安装指定版本
```
```cmd
npm install 包名 --save/-S   // 安装包信息将加入到dependencies(生产阶段的依赖)。先要npm init -y生成package.json配置文件
```
```cmd
npm install 包名 --save-dev/-D   // 安装包信息将加入到devDependencies(开发阶段的依赖)
```
- 2.卸载模块
```cmd
npm uninstall [包名|包名@版本] [-S|--save|-D|--save-dev]
```
- 3.更新模块
```cmd
npm update [-g] [包名]
```
- 4.清理本地缓存
```cmd
npm cache clean
```
##### 二、模块化详解
- 1.Node.js模块化机制之CommmonJS模块规范
每个文件就是一个模块，有自己的作用域，在一个文件里定义的变量、函数、类都是私有的，对其他文件不可见。其他文件引用采用require的方法。

- 2.ES6模块规范
学习地址：[https://es6.ruanyifeng.com/#docs/module]
ES6的module使用export 和 import来导出导入模块
