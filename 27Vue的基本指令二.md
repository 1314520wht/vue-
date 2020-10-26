
#### Vue的基本指令二
###### 6、v-model指令
v-model指令用来在input、select、text、checkbox、radio等表单控件元素上创建双向数据绑定。根据控件类型v-model自动选取正确的方法更新元素。尽管有点神奇，但是v-model不过是语法糖，在用户输入事件中更新数据。

###### 7、v-model指令详解
v-model会忽略所有表单元素的value、checked、selected特性的初始值而总是将Vue实例的数据作为数据来源。你应该通过JavaScript在组件的data选项中声明初始值。

v-model在内部为不同的输入元素使用不同的属性并抛出不同的事件：
1. text 和 textarea元素使用 value 属性和 input 事件
2. checkbox 和 radio 使用 checked 属性和 change 事件
3. select 字段将 value 作为 prop 并将change 作为事件

v-model指令修饰符：
1. number：如果想自动将用户的输入值转为数值类型，可以给 v-model添加number修饰符
2. trim：如果要自动过滤用户输入的首尾空白字符，可以给 v-model添加trim修饰符

###### 8、v-bind指令
v-bind指令用于响应更新HTML特性，将一个或多个attribute，或者一个组件prop动态绑定到表达式。

###### 9、v-for指令
我们可以用 v-for 指令基于一个数组来渲染一个列表。v-for 指令需要使用 (item,key,index) in items 形式的特殊语法，其中 items 是源数据数组，而 item 则是被迭代的数组元素的别名,key是当前项的键名,index是当前项的索引。

###### 10、v-for指令之数组更新检测
Vue 将被侦听的数组的变异方法进行了包裹，所以它们也将会触发视图更新。这些方法会改变vue实例里面的数组本身。这些被包裹过的方法包括：
1. push()
2. pop()
3. shift()
4. unshift()
5. splice()
6. sort()
7. reverse()

###### 11、v-for指令之注意点
**不要在同一元素上使用 v-if 和 v-for**
当它们处于同一节点，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。当如果你的目的是有条件地跳过循环的执行，那么可以将 v-if 置于外层元素 

###### 12、v-on指令
可以用 v-on 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。

###### 13、v-on指令之事件绑定
v-on 还可以接收一个需要调用的方法名称

除了直接绑定到一个方法，也可以在内联 JavaScript 语句中调用方法：直接调用并且传参

###### 14、v-on指令之事件修饰符
在事件处理程序中调用 event.preventDefault() 或 event.stopPropagation() 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

为了解决这个问题，Vue.js 为 v-on 提供了事件修饰符。之前提过，修饰符是由点开头的指令后缀来表示的。
.stop
.prevent
.capture
.self
.once
.passive  //这个属性就是BOM里面对事件监听的选项设置表示 listener 永远不会调用preventDefault()

使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 v-on:click.prevent.self 会阻止所有的点击，而 v-on:click.self.prevent 只会阻止对元素自身的点击。

###### 14、v-on指令之按键修饰符
在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：
你可以直接将 KeyboardEvent.key [https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values]暴露的任意有效按键名转换为 kebab-case 来作为修饰符。
常见的一些按键修饰符：
.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right

还有一些属于更加底层的按键, 不同的操作系统实现的功能可能会有些差异, 这些按键被称为系统按键
.ctrl
.alt
.shift
.meta
注意：在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 机器的键盘、以及其后继产品，比如 Knight 键盘、space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。

.exact 修饰符允许你控制由精确的系统修饰符组合触发的事件。

###### 15、v-on指令之按键修饰符
V-on指令可以给单个元素绑定多个事件,每个事件另起一个v-on指令即可

###### 16、v-once指令
通过前面的学习我们知道怎么将一个Vue实例中的data对象里的数据渲染到DOM元素中，但是如果我们只想在网页加载时，只渲染一次数据，后期即使是data种的数据变化了，我们也不要再次进行渲染，那么我们可以使用v-once指令。

###### 16、v-html指令
双大括号把数据解释为普通文本，而非HTML代码。所以在需要输出真正的HTML时，我们就可以使用v-html