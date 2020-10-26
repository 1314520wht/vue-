#### 0路由vue-router
官网地址:[https://router.vuejs.org/zh/]

1.创建路由组件

2.创建路由实例

3.在vue实例中，把路由实例挂载上去

4.在<div></div>中添加<router-view></router-view>让路由内容显示。

$route:用来获取url参数信息

router:设置路由信息

##### 一、路由简介
这里的路由并不是指我们平时所说的硬件路由器，这里的路由就是SPA（single page application）的路径管理器。vue的单页面应用是基于路由和组件的，路由用于设定访问路径，并将路径和组件映射起来。

传统的页面应用，是用一些超链接来实现页面切换和跳转的。在vue-router单页面应用中，则是路径之间的切换，也就是组件的切换。路由模块的本质 就是建立起url和页面之间的映射关系。

至于我们为啥不能用a标签，这是因为用Vue做的都是单页应用（当你的项目准备打包时，运行npm run build时，就会生成dist文件夹，这里面只有静态资源和一个index.html页面），所以你写的标签是不起作用的，你必须使用vue-router来进行管理。

##### 二、实现原理
单页面应用(SPA)的核心之一是: 更新视图而不重新请求页面;
vue-router在实现单页面前端路由时，提供了两种方式：Hash模式和History模式；根据mode参数来决定采用哪一种方式。

###### 1、vue-router实现原理之hash模式
hash 模式的原理是 onhashchange 事件(监测hash值变化)，可以在 window 对象上监听这个事件。

vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。hash（#）是URL 的锚点，代表的是网页中的一个位置，单单改变#后的部分，浏览器只会滚动到相应位置，不会重新加载网页，也就是说hash 出现在 URL 中，但不会被包含在 http 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面；

###### 2、vue-router实现原理之history模式
由于hash模式会在url中自带#，如果不想要很丑的 hash，我们可以用路由的 history 模式，只需要在配置路由规则时，加入"mode: 'history’”

这种模式充分利用了html5 history中新增的 pushState() 和 replaceState() 方法。这两个方法应用于浏览器记录栈，在当前已有的 back、forward、go 基础之上，它们提供了对历史记录修改的功能。只是当它们执行修改时，虽然改变了当前的 URL ，但浏览器不会立即向后端发送请求。

##### 三、vue-router激活路由样式(给点击的路由添加类名linkActiveClass)
给当前激活的菜单添加激活的样式
- 1. linkActiveClass
```js
let router = new Router({
    linkActiveClass:'custom-active-class'
})
```
- 2. linkExactActiveClass需要路由精确匹配才会添加到对应菜单上
```js
let router = new Router({
    linkExactActiveClass :'custom-exact-active-class'
})
```

##### 四、编程式导航(this.$router.push)

使用<router-link to:>比较麻烦，就可以用到编程式导航。实现路由直接的切换。

routes里面的path不能去掉

使用一个声明出来的固定元素来实现跳转局限性大，必须要有某个固定的触发元素, 为了实现更加柔性的跳转, 我们就引申出编程式的导航。
使用场景eg:在一个路由组件里面点击可以切换到另一个路由组件。

```js
// this.$router的基本方法
this.$router.push('目标路径') // 强制跳转至某一路径
this.$router.go(-1) //返回上一级
this.$router.replace('目标路径') // 路由替换
```

```js
// this.$router的方法
//1.当使用路由路径跳转的时候，用到path ,就要用query.它里面的参数会被拼接到路径里。
this.$router.push({path:"/foo2",query:{username:"xinxin"}})
//获取的时候用this.$route.query
routes里面的
 {path:"/foo1",component:foo1}
//2.当使用命名路由访问的时候，用到name,就要用params/query。它里面的参数会被拼接到路径里。
this.$router.push({name:"foo2", params:{name:"xinxin"}})
//当获取的时候用到this.$route.params/query  都可以
这个需要在routes里面也要写上name
{name:"foo1",path:"/foo1",component:foo1}
```

```
//同样，在router-link 中也可以使用path,query方法 。name,params方法
<router-link to="/demo1">go demo1</router-link>
<router-link :to="{name: 'demo1',params:{id:123}}">to 2</router-link>">
```

使用。

```
this.$router.push()
在组件中添加一个方法，把this.$router.push("要跳转的组件")
```

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第九期框架-佳楠</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.4.2/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-view></router-view>
    </div>

    <script>
        var Foo1 = {
            template: `<div>
                <button @click="toFoo2">go foo2</button>
                这是foo1的内容
            </div>`,
            methods: {
                // 定义一个编程式导航，去到foo2
                toFoo2() {
                    // this.$router.go(-1)
                    this.$router.push("/foo2")
                    // this.$router.replace("/foo2")
                }
            },
        }

        var Foo2 = {
            template: `<div>
                <button @click="toFoo1">go foo1</button>
                这是foo2的内容
            </div>`,
            methods: {
                // 定义一个编程式导航，去到foo1
                toFoo1() {
                    // this.$router.go(-1)
                    this.$router.push("/foo1")
                    // this.$router.replace("/foo1")
                }
            },
        }

        var router = new VueRouter({
            routes: [
                {path: "/foo1", component: Foo1},
                {path: "/foo2", component: Foo2}
            ]
        })

        let vm = new Vue({
            el: "#app",
            router
        })
    </script>
</body>
</html>
```

嵌套路由：在路由实例中使用children[ ]

要想访问demo11，则先输入demo1/输入id/demo11就可以访问到了。

```html
var router = new VueRouter({
	routes:[
		{
			path:"/demo1/:id",
			component:demo1,
			children:[
				{
					path:"demo11", //在子集中不用/
					component:demo11
				}
			]
		}
	]
})
要想让demo11的内容展示，则需要在demo1组件中添加<router-view></router-view>
```

完整代码

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第九期框架-佳楠</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.4.2/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <router-view></router-view>
    </div>

    <script>
        // 1、定义我们的路由组件
        const user = {
            template: `<div>
                <h1>User{{$route.params.id}}</h1>
                <router-view></router-view>
            </div>`
        }

        const demo1 = {
            template: `<div>
                <h1>demo</h1>
                <router-view></router-view>
            </div>`
        }

        const demo1Child = {
            template: `<div>
                <h1>demo1Child</h1>
            </div>`
        }

        // 2、创建router实例，然后传到routes路由配置
        var router = new VueRouter({
            routes: [
                {
                    path: "/user/:id", 
                    component: user,
                    children: [
                        {
                            path: 'demo1',
                            component: demo1,
                            children: [
                                {
                                    path: 'demo1Child',
                                    component: demo1Child
                                }
                            ]
                        }
                    ]
                }
            ]
        })

        let vm = new Vue({
            el: "#app",
            router
        })
    </script>
</body>
</html>
```

五。vue-router的初步使用

```js
// 1.创建vue实例
        let vm = new Vue({
            el:"#app",
            router
        })

```

```js
 // 2.创建路由组件
        var demo1 = {template:'<div>demo1</div>'}
        var demo2 = {template:'<div>demo2</div>'}
        var demo3 = {template:'<div>demo3</div>'}
        var demo4 = {template:'<div>demo4</div>'}
```

```js
// 3.创建路由实例
 let router = new VueRouter({
            routes:[
                {path:"/demo1",component:demo1},
                {path:"/demo2",component:demo2},
                {path:"/demo3",component:demo3},
                {path:"/demo4",component:demo4},
            ]
        })
```

```js
//4.使用
<div id="app">
        <p> //路由使用
           <router-link to="/demo1">go demo1</router-link>
           <router-link to="/demo2">go demo2</router-link>
           <router-link to="/demo3">go demo3</router-link>
           <router-link to="/demo4">go demo4</router-link>
        </p>
        <router-view></router-view>    //路由出口
</div>
```

完整

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.4.2/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <p>
           <router-link to="/demo1">go demo1</router-link>
           <router-link to="/demo2">go demo2</router-link>
           <router-link to="/demo3">go demo3</router-link>
           <router-link to="/demo4">go demo4</router-link>
        </p>
        <router-view></router-view>
    </div>

    <script>
        // 2.创建路由组件
        var demo1 = {template:'<div>demo1</div>'}
        var demo2 = {template:'<div>demo2</div>'}
        var demo3 = {template:'<div>demo3</div>'}
        var demo4 = {template:'<div>demo4</div>'}

        // 3.创建router实例
        let router = new VueRouter({
            routes:[
                {path:"/demo1",component:demo1},
                {path:"/demo2",component:demo2},
                {path:"/demo3",component:demo3},
                {path:"/demo4",component:demo4},
            ]
        })


        // 1.创建vue实例
        let vm = new Vue({
            el:"#app",
            router
        })

    </script>
    
</body>
</html>
```

### 六。路由重定向。

动态路由：：demo1/id 

1.重定向redirect 就是把路由重新定义一下，不用之前的了.用foo2代替foo1。会进入到foo2组件。

```
{name:"foo1",path:"/foo1",component:foo1，redirect:"/foo2"}
```

2.重命名alias就是把路由添加个路由名，可以通过别的路由名来访问。404页面，通过判断，如果不是某个确定的路由，则别的路由让他进入同一个页面

```
{name:"foo1",path:"/foo1",component:foo1,alias:"/cuihua"}
```

#### 七。动态路由获取参数。（$route.params）

```html
//在路由组件实例中。
var router = new VueRouter({
	routes:[{
		path:"/demo1/:id/:user",	
	}]
})
//在路由组件中,把动态路由的参数显示
var demo1 = {temlate:`<div>id:{{$router.params.id}}---user:{{$router.params.user}}</div>`}
```

```html
//完整
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第九期框架-佳楠</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.4.2/dist/vue-router.js"></script>
</head>
<body>
    <div id="app">
        <div>
            <router-link to="/demo1">go demo1</router-link>
            <router-link to="/demo2">go demo2</router-link>
            <router-link to="/demo3">go demo3</router-link>
            <router-link to="/demo4">go demo4</router-link>
        </div>
        {{this.$route.params.id}}
        {{this.$route.params.username}}
        <router-view></router-view>
    </div>

    <script>
        // 1、定义我们的路由组件
        var demo1 = {template: '<div>demo1</div>'}
        var demo2 = {template: '<div>demo2</div>'}
        var demo3 = {template: '<div>demo3</div>'}
        var demo4 = {template: '<div>id:{{$route.params.id}}user:{{$route.params.user}}</div>'}
        // 2、创建router实例，然后传到routes路由配置
        var router = new VueRouter({
            routes: [
                {path: "/demo1/:id", component: demo1},
                {path: "/demo2/:username", component: demo2},
                {path: "/demo3", component: demo3},
                {path: "/demo4/:id/:user", component: demo4}
            ]
        })

        let vm = new Vue({
            el: "#app",
            router
        })
    </script>
</body>
</html>
```

#### 八。嵌套路由

在路由实例中使用children

```html
 //通过user/id/demo1 可以访问到demo1的路由。
var router = new VueRouter({
            routes: [
                {
                    path: "/user/:id", 
                    component: user,
                    children: [
                        {
                            path: 'demo1',
                            component: demo1,
                            children: [
                                {
                                    path: 'demo1Child',
                                    component: demo1Child
                                }
                            ]
                        }
                    ]
                }
            ]
        })
```



#### 九。递归组件路由。

就是在组件模板中自己再调用自己。

比如二级导航，需要用到递归组件嵌套。

```html
//下面为二级导航，点击自己的时候，右侧显示出对应的组件内容
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第九期框架-佳楠</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.4.2/dist/vue-router.js"></script>
    <style>
        #app {
            display: flex;
            width: 100%;
        }
        #app .main {
            width: 80%;
            background: yellow;
        }
        #app a {
            text-decoration: none;
        }

    </style>
</head>
<body>
    <!-- 
        在组件内部可以嵌套调用其他组件，甚至可以递归调用当前组件，递归组件一定要有一个结束条件
        否则的话就会使组件不断进行循环引用，最终出现栈溢出的错误。
        所以我们使用v-if="false"作为递归组件结束的条件，组件则不再渲染。
     -->
    <div id="app">
        <menu-component :menus="menus"></menu-component>
        <main class="main">
            <router-view></router-view>
        </main>
    </div>

    <template id="menu-component">
        <ul>
            <li v-for="item in menus">
                <router-link :to="item.path">
                    {{item.name}}
                </router-link>
                <!-- 递归就是调用本身 -->
                <menu-component v-if="item.children" :menus="item.children"></menu-component>
            </li>
        </ul>
    </template>

    <script>
        // 1、定义路由组件
        var hunan = {template: "<div>湖南省</div>"}
        var hubei = {template: "<div>湖北省</div>"}
        var huanggang = {template: "<div>黄冈市</div>"}
        var wuhan = {template: "<div>武汉市</div>"}
        var hongshan = {template: "<div>洪山区</div>"}
        var jianghan = {template: "<div>江汉区</div>"}
        var hebei = {template: "<div>河北省</div>"}

        // 2、创建router实例
        var router = new VueRouter({
            routes: [
                {path: "/hunan", component: hunan},
                {path: "/hubei", component: hubei},
                {path: "/huanggang", component: huanggang},
                {path: "/wuhan", component: wuhan},
                {path: "/hongshan", component: hongshan},
                {path: "/jianghan", component: jianghan},
                {path: "/hebei", component: hebei},
            ]
        })

        // 创建菜单组件
        const MenuComponent = {
            name: "MenuComponent",
            template: "#menu-component",
            props: ['menus'] // 菜单数据
        }

        // 创建vue实例
        const app = new Vue({
            el: "#app",
            router,
            data: {
                menus: [
                    {
                        name: "湖南省",
                        path: "/hunan"
                    }, {
                        name: "湖北省",
                        path: "/hubei",
                        children: [{
                            name: "黄冈市",
                            path: "/huanggang"
                        }, {
                            name: "武汉市",
                            path: "/wuhan",
                            children: [{
                                name: "洪山区",
                                path: "/hongshan"
                            }, {
                                name: "江汉区",
                                path: "/jianghan"
                            }]
                        }]
                    }, {
                        name: "河北省",
                        path: "/hebei"
                    }
                ]
            },
            components: {
                MenuComponent
            }
        })
    </script>

</body>
</html>
```

