# 第二课pc,移动端,h5事件

## 1.pcH5事件对比

### 01.PC、移动端事件简单对比

####  （1）pc端事件：

onclick鼠标点击触发
onmousedown鼠标按下触发
onmousemove鼠标移动触发
onmouseup鼠标抬起触发

```html
<div id="box"></div>
    <script>
        let box = document.querySelector('#box')
        // PC端事件 按下 移动 抬起鼠标
        // 按下
        box.onmousedown = function(e) {
            console.log(e)
            console.log('pc按下')
            // 移动
            box.onmousemove = function(e) {
                console.log('pc移动')
            }
            // 抬起
            box.onmouseup = function(e) {
                console.log('pc抬起')
                // 此时要清除一下我们的移动和抬起事件
                box.onmousemove = box.onmouseup = null;
            }
        }
	</script>

```

####  （2）移动端事件：

ontouchstart手指按下触发
ontouchmove手指移动触发
ontouchend手指抬起触发

```js
 // 移动端事件 手指按下 手指移动 手指抬起
        // 手指按下
        box.addEventListener('touchstart', function(e) {
            console.log('移动端-我的手指按下了')
        })

        // 手指移动
        box.addEventListener('touchmove', function(e) {
            console.log('移动端-我的手指移动了')
        })

        // 手指抬起
        box.addEventListener('touchend', function(e) {
            console.log('移动端-我的手指抬起了')
        })
```

#### （3）部分小点

1.另外：通过on的事件为0级事件，如果有相同事件出现则会发生事件覆盖的情况。而通过事件监听addEventListener2级事件则不会覆盖，项目开发中用二级事件。

2.通过on的方式添加touch事件在谷歌浏览器下无效

3.鼠标事件在移动端可以使用，但是会有300ms的延迟

ontouchstart事件会先与onmousedown执行。

 一般我们使用fastclick.js解决移动端浏览器300ms延迟的问题

 它是专门为解决移动端浏览器300ms延迟点击问题所开发的一个轻量的库

 fastclick.js在检测到touched事件的时候，会通过dom自定义事件，然后立即触发一个

 模拟的click事件，并把浏览器在300ms之后真正的click事件阻止掉

4.addEventListener事件绑定多少个事件它就执行多少个事件，并且在谷歌浏览器中一直被识别

### 02.阻止默认事件（e.preventDefault）

  比如在移动端手指按下时阻止默认的事件

```js
document.addEventListener("touchstart",function(){
  e.preventDefault()
})
```

冒泡：由内而外

捕获：由外到里

当同时存在事件冒泡和捕获时，捕获优先级高于冒泡

#### 1.阻止冒泡（e.stopPropagation( )）

​    阻止点击时选中文本：css操作：user-select:none

​                                            js操作：document.onselectstart= function(){return false}; 

### 03.获取手指信息

touches当前屏幕上的手指列表
targetTouches当前元素上的手指列表
changedTouches触发当前事件的手指列表
获取手指的个数 e.changedTouches.length
获取坐标e.changedTouches[0].pageX && e.changedTouches[0].pageY

```html
<div id="app"></div>
    <script>
        let box = document.querySelector('#app')

        box.addEventListener('touchstart', function(e) {
            console.log(e)
        })
    </script>
```



##### 手指对象的区别

在touchend的时候想要获取手指列表，只能用changedTouches
因为手指抬起了，也就没有touches，targetTouches了，只能用changedTouches

### 04.获取设备信息（navigator）

 navigator 获取用户浏览器信息(UA:User-Agent)

navigator.appName; // 返回浏览器的名称

 navigator.appCodeName; // 返回浏览器的代码名。

 navigator.appVersion; // 返回浏览器的平台和版本信息。

 navigator.userAgent; // 返回由客户机发送服务器的 user-agent 头部的值。

 navigator.cookieEnabled; // 返回指明浏览器中是否启用 cookie 的布尔值。

 navigator.onLine; //返回指明系统是否处于脱机模式的布尔值

 navigator.platform; // 返回运行浏览器的操作系统平台

 navigator.language; // 返回 OS 的自然语言设置

```html
<div id="app"></div>
    <script>
        let app = document.querySelector('#app')

        app.addEventListener('touchstart', function(e) {
            // navigator 获取用户浏览器信息(UA:User-Agent)
            let userAgentInfo = navigator
            console.log(userAgentInfo)
        })
        </script>
```

## 2.js，移动端，H5拖拽

要想拖动，首先盒子需要绝对定位

### 01.js拖拽

onmousedown，onmousemove，onmouseup

```html
<div id="app"></div>
    <script>
        let app = document.querySelector('#app')

        app.onmousedown = function(e) {
            let x = e.clientX - this.offsetLeft //鼠标点击的位置-盒子距离定位父级的位置
            let y = e.clientY - this.offsetTop
            app.onmousemove = function(e) {
                app.style.cssText = `left:${e.clientX - x}px;top:${e.clientY - y}px`
            } //下次的位置-上次的位置
            app.onmouseup = function() {
                app.onmousemove = onmouseup = null  //最后把移动和抬起事件清空
            }
        }
    </script>
```

### 02.H5拖拽（超简单）

```html
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        #app {
            width: 200px;
            height: 200px;
            background: yellow;
            position: absolute;
        }
    </style>
</head>
<body>
    <div id="app" draggable='true'></div>
    <!-- 
        draggable是HTML5的新特性，为了让元素可以拖动
        draggable属性：值是有true|false|auto
        auto是根据浏览器定义是否可以拖动
        一般图片是默认看看可以拖拽的，所以它的draggable属性默认是true
    -->
    <script>
        let app = document.querySelector('#app')
        
        app.ondragend = function(e) {
            this.style.cssText = `left:${e.clientX}px;top:${e.clientY}px`
        }
    </script>
</body>
```

## 3.H5七事件

拖拽元素事件

ondragstart  拖拽开始的事件

ondrag  拖拽过程中持续触发的事件

ondragend  拖拽结束的事件

拖拽的目标元素事件

ondragenter  元素被拖拽进入目标元素时触发

ondragover  在目标元素移动停留时

ondragleave  元素被拖拽离开目标元素时触发

ondrop  在目标元素上面松开鼠标时触发

比如做个类似于文件拖拽进回收站的案例

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
            display: flex;
            justify-content: space-between;
        }
        #target {
            width: 300px;
            height: 300px;
            background: url('img/bg.jpg') center center no-repeat;
            border: 1px solid #ccc;
        }
        #drag {
            width: 200px;
            height: 200px;
            background: yellow;
        }
    </style>
</head>
<body>
    <div id="target"></div>
    <div id="drag" draggable="true"></div>
    <script>
        // 一、拖拽元素事件
        // 1、拖拽开始的事件
        drag.ondragstart = function() {
            this.style.background = 'red'
        }
        // 2、拖拽过程中持续触发的事件
        drag.ondrag = function() {
            console.log('我正在被移动')
        }
        // 3、拖拽结束的事件
        drag.ondragend = function() {
            this.style.background = 'pink'
        }

        // 二、拖拽的目标元素事件
        // 4、元素被拖拽进入目标元素时触发
        target.ondragenter = function() {
            this.innerText = '请释放鼠标删除'
        }
        // 5、在目标元素移动停留时
        target.ondragover = function(e) {
            this.innerText = '在目标区域游走'
            e.preventDefault()
        }
        // 6、元素被拖拽离开目标元素时触发
        target.ondragleave = function() {
            this.innerText = '拖动至此处删除'
        }
        // 7、在目标元素上面松开鼠标时触发
        target.ondrop = function() {
            document.body.removeChild(drag)
            this.innerText = '文件已删除'
        }
    </script>
</body>
</html>
```

### 4.常用属性之文件信息

// e.dataTransfer.files获取文件信息

```js
target.ondragover = function(e) {
            this.innerText = '松开鼠标删除图片'
            e.preventDefault()
        }

        target.ondrop = function(e) {
            // e.dataTransfer.files获取文件信息
            console.log(e.dataTransfer.files)
            e.preventDefault()// 不阻止默认的话，就会执行浏览器默认的打开图片进行预览
        }
```

### 5.fileReader读取文本文件

```js
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
        #target {
            width: 300px;
            height: 300px;
            border: 1px solid #ccc;
        }
    </style>
</head>
<body>
    <div id="target"></div>
    <script>
        target.ondragover = function(e) {
            this.innerText = '松开鼠标读取文件内容'
            e.preventDefault()
        }
        target.ondrop = function(e) {
            const fileInfo = e.dataTransfer.files
            e.preventDefault()
            var file = fileInfo[0]
            var reader = new FileReader() // 创建一个文件读取对象实例
            // reader.readAsBinaryString() 使用原始二进制字符串来展示文件数据
            // reader.readAsArrayBuffer() 使用指定的二进制对象来读取内容
            // reader.readAsText() 使用文本字符串来展示文件数据
            // reader.readAsDataURL() 使用URL来展示文件数据

            console.log(reader)
            reader.readAsText(file)
            reader.onloadend = function(e) {
                /*              未加载任何数据  |   数据正在被加载  | 完成所有的读取请求
                    readyState       0                  1                   2
                    FileReader      EMPTY              LOADING             DONE
                */
                if (e.target.readyState === FileReader.DONE) {
                    let result = reader.result
                    console.log(result)
                    target.innerHTML = 'File:' + file.name + result
                }
            }
        }
    </script>
</body>
</html>
```

### 6.filderReader读取图片

```js
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
        #target {
            width: 400px;
            height: 400px;
            border: 1px solid #ccc;
        }
    </style>
</head>
<body>
    <div id="target"></div>
    <script>
        target.ondragover = function(e) {
            this.innerText = '松开鼠标读取文件内容'
            e.preventDefault()
        }
        target.ondrop = function(e) {
            const fileInfo = e.dataTransfer.files  //文件信息
            e.preventDefault()
            console.log(fileInfo)  //FileList{0:File,length:1}
            console.log(1)
            var file = fileInfo[0]
            console.log(file)
            var reader = new FileReader() // 创建一个文件读取对象实例

            console.log(reader) 
            reader.readAsDataURL(file) // 会以数据URI格式返回,所以类似于src属性

            reader.onload = function(e) {
                const img = new Image()
                console.log(reader.result)
                // 读取图片时，reader.result返回一个base64编码的图片路径
                img.src = reader.result
                img.width = 300
                img.height = 250
                target.appendChild(img)
            }
        }
    </script>
</body>
</html>
```

### 7.blob读取文件

```js
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
        #target {
            width: 400px;
            height: 400px;
            border: 1px solid #ccc;
        }
    </style>
</head>
<body>
    <div id="target"></div>
    <script>
        target.ondragover = function(e) {
            this.innerText = '松开鼠标读取文件内容'
            e.preventDefault()
        }
        target.ondrop = function(e) {
            e.preventDefault()
            this.innerText = '文件读取成功'
            const fileInfo = e.dataTransfer.files
            let file = fileInfo[0]
            console.log(file)

            let blob = new Blob([file]) // 第一个参数一定是一个数组,第二个是文件类型
            // let blob = new Blob([file], {type: 'image/jpeg'}) 
            let url = window.URL.createObjectURL(blob)
            
            console.log(url)
            let img = new Image()
            img.width = 300
            img.height = 300
            img.src = url
            img.onload = function() {
                target.appendChild(this)
            }

        }
    </script>
</body>
</html>
```

