#### vue-router进阶
##### 一、命名路由
之前我们已经学习过,用this.$router.push方式可以实现编程式路由,push方法除开可以传入字符串的参数外,还可以传入一个复杂的对象值。
```js
this.$router.push("/home") // 字符串
this.$router.push({ path: 'home' }) // 对象
this.$router.push({ path: 'home' , query: {id : '123'}}) // 带查询参数就变成/home?id=123
const userId = 123
this.$router.push({ path: `home/${userId}`}) //  /home/123
this.$router.push({ name: 'home', params: {id: '123'} }) //  /home/123
```
注意：如果使用path,params会被忽略

```
1.path和query的使用
// 1、使用path跳转时传递参数用query对象，获取时用this.$route.query。如果使用了params则会被忽略掉，导致参数无法传递
this.$router.push({ path: "/foo1", query: {userName: 'jianan', id: '123'} })
```

```
2.name和params的使用
使用name命名式路由跳转的时候，路由要定义名字，传参时使用params/query，获取时this.$route.params/query
  this.$router.push({ name: 'foo1', params: {name: name1} })
```



```html
//命名路由1
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
                toFoo2() {
                    this.$router.push("/foo2")
                }
            },
            created() {
                console.log(this.$route.query)
                console.log(this.$route.params)
            }
        }

        var Foo2 = {
            template: `<div>
                <button @click="toFoo1">go foo1</button>
                这是foo2的内容
            </div>`,
            methods: {
                toFoo1() {
                    // this.$router.push("/foo1")
                    // 1、使用path跳转时传递参数用query对象，获取时用this.$route.query。如果使用了params则会被忽略掉，导致参数无法传递
                    // this.$router.push({ path: "/foo1", query: {userName: 'jianan', id: '123'} })
                    const name1 = 'hahaha'
                    // this.$router.push({path: `foo1/${name}`})
                    // 2、使用name命名式路由跳转的时候，路由要定义名字，传参时使用params/query，获取时this.$route.params/query
                    this.$router.push({ name: 'foo1', params: {name: name1} })
                }
            }
        }

        var router = new VueRouter({
            routes: [
                {name: 'foo1', path: "/foo1", component: Foo1},
                // {path: "/foo1/:name", component: Foo1},
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



```html
//命名路由2
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
// 链接到一个命名路由，可以给router-link 的to属性传递一个对象
// 这个跟编程式路由跳转，即用代码跳转传递参数是一回事
        var Foo1 = {
            template: `<div>
                <button @click="toFoo2">go foo2</button>
                <router-link :to="{name: 'foo2', params:{id: 123}}">登录</router-link>
                这是foo1的内容
            </div>`,
            methods: {
                toFoo2() {
                    this.$router.push({ name: 'foo2', params: {userId: '123333'} })
                }
            }
        }

        var Foo2 = {
            template: `<div>
                <button @click="toFoo1">go foo1</button>
                这是foo2的内容
            </div>`,
            methods: {
                toFoo1() {
                    this.$router.push("/foo1")
                }
            },
            created() {
                console.log(this.$route.params)
            }
        }

        var router = new VueRouter({
            routes: [
                {name: 'foo1', path: "/foo1", component: Foo1},
                {name: 'foo2',path: "/foo2", component: Foo2}
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



##### 二、重定向（ redirect）和重命名（alias）
重定向的意思是，当用户访问/a的时候，url可以被替换成/b。进入b的路由。

```html
 {name: 'foo1', path: "/foo1", component: Foo1, redirect: '/foo2'},
 // 重定向：用后面的路由代替前面的路由 redirect
```

重命名是访问路由/a时可以设置一个别名/d,那么当用户访问/d的时候访问的也是/a的页面

```html
{name: 'foo2',path: "/foo2", component: Foo2, alias: '/goudan'}
 // 重命名：两个路由名度可以用  alias
```



##### 三、路由的导航守卫
vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中：全局的, 单个路由独享的, 或者组件级的。

每个守卫方法接收三个参数：
- to: Route: 即将要进入的目标 路由对象

- from: Route: 当前导航正要离开的路由

- next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。
  如果next函数没有执行或是传入了false等值,这个跳转就会被终止掉

  

###### 1、全局前置守卫
当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 等待中。
```js
// 可以使用 router.beforeEach 注册一个全局前置守卫
//通过判断path，来进行操作.如果进入的路由不是foo1，那么就让他进去foo2中
const router = new VueRouter({ routes:[{name:"demo1",path:"/demo1",component:  ,}]... })

router.beforeEach((to, from, next) => {
  // ...
    //next()
    if(to.path !== '/foo1'){
	next({name:'foo2'})
}
next()
})
```

###### 2、路由独享守卫
```js
// 可以在路由配置上直接定义 beforeEnter 守卫
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...next() //当不给next的时候，就访问不了这个路由。
      }
    }
  ]
})
```

###### 3、组件内的守卫
```js
// 可以在路由组件内直接定义以下路由导航守卫
// beforeRouteEnter
// beforeRouteUpdate
// beforeRouteLeave
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
      要想拿到组件实例this的话，需要在next中回调
      next(vm=>{
          console.log(vm)  可以通过vm获取this. 只有在这里可以用
      })
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用.动态路由。
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
      比如在离开路由的时候，问一下是否要离开。但是如果直接用户关闭页面的话是没办法的。
      const answer = window.confirm("你确定要离开吗")
      if(answer){
          next()
      }else{
          next(false)
      }
  }
}
```

###### 4、完整的导航解析流程
1. 导航被触发。
2. 在失活的组件里调用 beforeRouteLeave 守卫。
3. 调用全局的 beforeEach 守卫。
4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
5. 在路由配置里调用 beforeEnter。
6. 解析异步路由组件。
7. 在被激活的组件里调用 beforeRouteEnter。
8. 调用全局的 beforeResolve 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 afterEach 钩子。
11. 触发 DOM 更新。
12. 调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入。+】
