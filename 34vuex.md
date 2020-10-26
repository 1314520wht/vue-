#### vuex

#### 创建一个项目，使用脚手架。

状态管理模式。采用集中式存储管理应用的所有组件的状态。

vuex适用与中，大型项目，小型项目不适用

脚手架下载完成之后，开始下载vuex。npm install -S vuex

使用如下：

```js
//在main.js中引入store.js文件  import {store} from './store'
//1.在src目录下新建一个store.js文件是个代码仓库
//在store.js中首先要引入vue 和vuex。然后引入了还得用use挂载一下。然后导出
import Vue from 'vue'
import Vuex from 'vuex'  //第一引入

Vue.use(Vuex)  //第二需要use一下

export const store = new Vuex.Store({  
    state: {}, //相当与data
    getter:{}, //相当于computed
    mutations:{}, // 相当于methods
    actions:{} // 异步操作，提交mutatins
})


//使用的时候.$store.state.属性
//  {{计算属性}}  this.$store.getter.计算属性
```

```js
//2.在main.js文件中引入store.js导出的仓库   
import { store} from './store'
//3.在main.js实例中需要挂载一下这个仓库
store,
```

父子组件传值。

需求。有一个父组件（home）有两个子组件，分别是One和Two.父组件中包含两个子组件。

新建Home.vue组件，One，Two组件。引入辅助函数。

```js
//3.在这里使用的话需要导入以下内容,在script中
//对应组件中使用
import {mapState,mapGetters, mapMutations,mapActions} from 'vuex' 。
然后export default {
  data(){
      return
  },
  computed:{  //直接映射过来，就可以使用。在store中定义的
      ...MapState(["count","friend"]), //然后需要啥数据，就在这里添加。使用
      ...mapGetters(["changeAge","changeList"]),
  },
  methods:{
      //使用辅助函数
      ...mapMutations(["newAge"]),在store.js已经
      //这两种作用一致.有两种用法
      //使用commit。在方法中使用如：
//       newAge(){
//           this.$store.commit('newAge',30)
//       }
//   }
    //使用辅助函数
     ...mapActions(["newAgeAsync"]),
     //使用dispach
     newAgeAsync (){
         this.$store.dispach('newAgeAsync',2)
     }
  }
}
```

##### 完整代码

```js
//store.js
// 多个组件使用共同的转态
// 在代码库中要使用vue和vuex就业引入vue和vuex
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex) // 引入之后这里还得使用一下

// 把代码库导出 store.jsa
export const store = new Vuex.Store({
  // 定义第一个state，类似于vue实例中的data
  state: {
    count:0,
    friend:[
      {name:"兴",age:"18"},
      {name:"李",age:"19"},
      {name:"婷",age:"20"},
      {name:"猪",age:"21"},
    ]
  },
  getter: {
    changeAge:(state)=>{
      state.friend.map(item=>{
        return {
          name:item.name,
          age:item.age + 10
        }
      })
    },
    changeList:(state)=>{
      state.friend.map(item=>{
        return {
          name:"变成老年人" + item.name,
          age:item.age + 100
        }
      })
    }
  },
  mutations: {
    newAge:(state,cuihua)=>{
      return state.friend.map(item => {
        item.age -= cuihua
      })
    }
  },
  actions: {
    newAgeAsync:(storeAll,goudan)=>{
      setTimeout(()=>{
        storeAll.commit('newAge',goudan)
      })
    }
  }
})



//使用模块化 在公共仓库store.js中 定义数据，方法，并导出
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
const moduleA = {
  state:{},
  getters:{},
  mutations:{},
  actions:{}
}
const moduleB = {
  state:{},
  getters:{},
  mutations:{},
  actions:{}
}
export const store = new Vuex.store({
  modules:{
    moduleA:moduleA,
    moduleB:moduleB
  }
})
//使用的时候。mapState  mapGetters 放到data(){return 里面} ...mapState(["定义的内容"])
//在mapSMountain里面定义时： goudan:(()=>{})


//state是所以组件的公共data
//getters是对于state中派生的一些转态。相当于computed，getter的返回值会根据它的依赖被缓存起来，并且只有当它的依赖值发送了改变的时候才会重新计算
// 更改vuex中state中状态的唯一方法是提交mutations，vuex中mutations类似于事件

// action类似于mutation，不同之处在于
// action是提交mutation,而不是直接改变状态
// action可以包含异步操作。但是mutation是不行的
// action是通过$store.dispath方法去触发的，mutation通过$store.commit方法提交

// 当我们的应用所有的状态集中到一个store中的时候，应用变得比较复杂，为了解决这个问题
// Vuex允许我们将store分割成模块(module),每个模块拥有自己的state、getter、mutation、action

```

```js
//Helloworld.vue
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

```

```js
//one.vue
<template>
    <div>
        我是子组件one页面
        <p>{{count}}</p>
        <p>{{$store.state.count}}</p> <!--在这里不使用this 在script标签中使用this-->
        <ul>
            <!-- 用到state里的数据 -->
            <!-- <li v-for="(item,index) in $store.state.friend" :key='index'> -->
            <!-- <li v-for="(item,index) in friend" :key='index'>     -->

            <!-- 使用getters里的数据 -->
            <li v-for="(item,index) in changeAge" :key='index'>     
                <span>姓名：{{item.name}}</span>
                <span>年龄：{{item.age}}</span>
            </li>
            <li v-for="(item,index) in changeList" :key='index'>     
                <span>姓名：{{item.name}}</span>
                <span>年龄：{{item.age}}</span>
            </li>
            <!-- 使用mutations里的数据 -->
            <button @click="newAge(5)">可以减低年龄欧</button>
            <!-- 使用actions里的数据 -->
            <button @click="newAgeAsync(3)">延迟3秒减低年龄</button>
        </ul>
    </div>
</template>>

<script>
import {mapState,mapGetters, mapMutations,mapActions} from 'vuex'  //辅助函数，如果需要获取多个state。可以使用mapState  先在这里引入state
export default {
  data() {
      return {
        // count:this.$store.state.count,
        // friend:this.$store.state.friend  //但是用这种方法比较麻烦，可以用辅助函数来拿到仓库里的数据...Map
      }
  },
  computed:{
      ...MapState(["count","friend"]), //然后需要啥数据，就在这里添加。使用
      ...mapGetters(["changeAge","changeList"]),
      
  },
  methods:{
      //使用辅助函数
      ...mapMutations(["newAge"]),
      //这两种作用一致
      //使用commit
//       newAge(){
//           this.$store.commit('newAge',30)
//       }
//   }
    //使用辅助函数
     ...mapActions(["newAgeAsync"]),
     //使用dispach
     newAgeAsync (){
         this.$store.dispach('newAgeAsync',2)
     }
}
}
</script>

<style scoped>
</style>
```

```js
//two.vue
<template>
    <div>
        <p>我是子组件two页面</p>
        <button @click="$store.state.count++"></button>
        <button @click="add"></button>
    </div>
</template>>

<script>
import {mapState} from 'vuex'
export default {
    data() {
        return {
            add(){
                this.$store.state.count++
            }
        }
    },
    computed:{
        ...mapState(["count","friend"])
    }
}
</script>

<style scoped>

</style>
```

