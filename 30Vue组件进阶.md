#### Vue组件进阶
##### 一、组件注册
###### 1、组件注册之组件名称要求
- 用于展示类、无逻辑、无状态的组件应该全部以一个特定的前缀开头，比如App、Base等。
如：BaseButton.vue、BaseTable.vue、AppButton.vue等。
这样做的好处是，让你的代码看起来更加直观，方便进行模块划分。

##### 二、组件属性
###### 1、组件的属性props之属性名
props中的prop如果使用了驼峰命名，那么在HTML中要使用-短横线分隔命名。
###### 2、props之prop类型定义
定义类型的好处是，可以给组建的prop指定验证要求,如果传递的值没有满足对应的类型的话，则Vue会在浏览器的控制台警告你，这样有助于有人协作开发。
```js
props: {
    title: String,
    likes: Number,
    state: Boolean,
    ids: Array,
    author: Object,
    callback: Function,
}
    //就是把数组换为对象，加上他的类型。
```

##### 三、自定义事件(dir)
自定义事件之事件名称，不同于组件和prop,事件名不应存在任何自动化的大小写转换。而是触发的事件名需要完全匹配监听这个事件所用的名称。
v-on 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)
**自定义事件名称必须要用全小写或者-分隔的形式**



##### 四、插槽
```html
<slot></slot>
<!-- Vue实现了一套内容分发的API，将slot元素作为承载分发内容的出口。 -->
```
1. 插槽的基本用法：
使用注册组件时，直接在组件标签中写入内容是无效的，但是如果在组件里面使用了slot标签的话，则能够将内容展示出来。

2. 插槽的编译作用域：
父级模板里的所有内容都是在父级作用域中编译的，子模 板里的所有内容都是在子作用域中编译的。

3. 插槽的后备内容：
为插槽设置具体的后备内容(也就是默认的内容)很有用，它只会在没有提供内容的时候被渲染。

4. 具名插槽：
有时我们需要多个插槽 , slot 元素有一个特殊的特性：name。这个特性可以用来定义额外的插槽：一个不带 name 的 slot 出口会带有隐含的名字“default”。

在向具名插槽提供内容的时候，我们可以在一个 template 元素上使用 v-slot 指令，并以 v-slot 的参数的形式提供其名称：任何没有被包裹在带有 v-slot 的 template 中的内容都会被视为默认插槽的内容。

#### 五。extend使用 (vue全局方法，extend和$nextTick)

Vue.extend({}) 与组件一个意思。他是一个类，需要实例化一下.可以实现多次调用

比如：1.

```
let People = Vue.extend({
	name:"类",
	template:"",
	data(){
	  return {
	    
	  }
	}
}）
new People().$mount("#app")

//相当于下面
new People({el: "#app2"})
这样来使用
```

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第九期框架-佳楠</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app"></div>
    <div id="app1"></div>
    <div id="app2"></div>
    <div id="app3"></div>
    <script>
        // 使用Vue构造器，创建一个"子类",参数是一个包含组件选项的对象
        // data选项是特例，在我们Vue.extend里必须是一个函数
        let People = Vue.extend({
            template: '<p>{{name}}---{{age}}</p>',
            data: function() {
                return {
                    name: "jianan",
                    age: 15
                }
            }
        })
        // 创建People实例，并且挂载到了元素上
        new People().$mount("#app")
        new People().$mount("#app1")
        // 同上
        new People({el: "#app2"})
        // 或者在文档之外渲染并且随后挂载
        let component = new People().$mount()
        document.getElementById("app3").appendChild(component.$el)

    </script>
</body>
</html>
```

#### 六。$nextTick 

```html
// 在下次DOM更新循环结束之后执行延迟回调，在修改数据之后立即使用这个方法，可以获取最新的DOM。比如beforemounted中，原本是拿不到el中后来的数据，但是使用了$nextTick就可以拿到了

```

```html
//1.在全局下使用
Vue.nextTick(function() {
            console.log('数据更新了')
        })

<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第九期框架-佳楠</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app"></div>
    <div id="app1"></div>
    <script>
       
        let People = Vue.extend({
            template: '<p>{{name}}---{{age}}</p>',
            data: function() {
                return {
                    name: "jianan",
                    age: 15
                }
            }
        })
        // 创建People实例，并且挂载到了元素上
        let el = new People().$mount("#app")
        el.name = "更新name的操作"
        // 在下次DOM更新循环结束之后执行延迟回调，在修改数据之后立即使用这个方法，可以获取最新的DOM
        Vue.nextTick(function() {
            console.log('数据更新了')
        })

    </script>
</body>
</html>
```

```html
  2.在实例中使用
  <!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第九期框架-佳楠</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{message}}
        <button @click="change">改变message</button>
    </div>
    <script>
        // 它跟全局方法Vue.nextTick一样，不同的是回调的this自动绑定到调用它的实例上面的。
       const vm = new Vue({
           el: "#app",
           data: {
               message: "hello"
           },
           methods: {
               change() {
                   this.message = "changed"
                   this.$nextTick(function() {
                       console.log("已经更改")
                   })
               }
           },
           methods: {
            init() {
                // 这是项目初始化的函数
            }
           },
           created() {
            // this.init()
            // 可以拿到最新的DOM，根据当前生命周期所做的操作，来做一个回调，回调执行完毕之后，就可以得到最新的DOM
               this.$nextTick(function() {
                    // 始终可以提前拿到最新的值
                    this.message = "create changed2"
                    console.log("已经更改")
                    console.log(this.$el)
                    this.message= '323'
               })
           },
           beforeMount() {
                console.group("--Vue实例对象和文档挂载之前beforeMount--")
                console.log("%c%s", "color: red", "el:   " + this.$el, this.$el.innerHTML)
                this.message = "changed1"
                // 使用它之后就可以拿到原本只能在Mounted里面拿到的值
                this.$nextTick(function() {
                    // 始终可以提前拿到最新的值
                    this.message = "changed2"
                    console.log("已经更改")
                    console.log(this.$el)
                })
           }
        //    mounted() {
        //         console.group("--Vue实例对象和文档挂载之后mounted--")
        //         console.log("%c%s", "color: red", "el:   " + this.$el, this.$el.innerHTML)
        //         console.log("%c%s", "color: red", "data: " + this.$data)  
        //    }
       })

    </script>
</body>
</html>
```

#### 七。动态组件

在component里面使用 ：is      //<!-- is特性绑定哪个组件就显示哪个组件 -->

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>第九期框架-佳楠</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <style>
        .tab {
            border: 1px solid #ccc;
            padding: 30px;
        }
        .tab-button {
            padding: 6px 10px;
            border: 1px solid #ccc;
            cursor: pointer;
        }
        .tab-button:hover {
            background: #e0e0e0;
        }
        .tab-button.active {
            background: #e0e0e0;
        }
    </style>
</head>
<body>
    <!-- 有的时候，在不同组件之间进行动态切换是很常用的
        比如在一个新闻网站实现多新闻内容同选项卡
        那么此时就可以使用Vue的<component>元素加一个特殊的is特性来实现
    -->
    <div id="app1">
        <button
            v-for="tab in tabs"
            :key="tab"
            @click="change(tab)"
            v-bind:class="['tab-button', {active: currentTab === tab}]">
        {{tab}}
        </button>
        <!-- is特性绑定哪个组件就显示哪个组件 -->
        <component :is="currentTabComponent" class="tab"></component>
    </div>
    <script>
        Vue.component('tab-home', {
            template: `<div>Home Page</div>`
        })
        Vue.component('tab-list', {
            template: `<div>List Page</div>`
        })
        Vue.component('tab-news', {
            template: `<div>News Page</div>`
        })

        let vm1 = new Vue({
            el: "#app1",
            data: {
                tabs: ['home', 'list', 'news'],
                currentTab: 'home'
            },
            computed: {
                currentTabComponent() {
                    return 'tab-' + this.currentTab
                }
            },
            methods: {
                change(val) {
                    this.currentTab = val
                }
            },
        })
    </script>
</body>
</html>
```

