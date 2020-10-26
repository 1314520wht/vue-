### 一、JQuery了解

#### 1 .原生JS的问题

我们会发现原生的JS编程很麻烦,尤其在兼容性的问题

> 1. 选择元素,权限兼容的只有getElementById和getElementsByTagName;其他方法都有兼容问题
> 2. 样式操作也有兼容问题,还得我们自己封装，封装getStyle（）
> 3. 动画也麻烦,也得自己封装，封装animation（）
> 4. HTML节点操作也挺麻烦的

JS里面麻烦的都是和DOM编程有关的,有兼容问题,还的我们自己封装.

jQuery是原生JS封装的,简化了JS的DOM编程，完美的解决了原生js在DOM操作上的难题



#### 2. JQuery

口号:

​	写更少的代码，做更多的事情

官方自我介绍:

​	jQuery是一个快速、小型的、特性很多的JS库，它把很多事儿都变得简单。jQuery是免费的、开源的。



版本线：

​	1.x版本

​	2.x版本

​	3.x版本



jQuery分压缩版和未压缩版



### 二、 $()   选择器

$ 就是jQuery的核心，query就是选择的意思

> ##### 语法
>
> $(‘选择器’)，jQuery(‘选择器’)
>
> $可以用jQuery代替，$和jQuery是同一个函数
>
> 选中某个或某类元素

```html
<style type="text/css">
    p {
        float:left;
        width: 60px;
        height: 60px;
        background-color: #ccc;
        margin-left: 10px;
        margin-top: 10px;
    }
</style>

<p></p>
<p></p>
<p></p>
<p></p>

<script src="jQuery.js"></script>
<script>
    console.log($)
    $('p').css('background-color','red');
</script>
```



#### 1. $() 选择出来的是一个类数组

哪怕选择的是一个单独的元素,返回的也是一个类数组0

```js
console.log($('#box'));
// [div#box]
```

既然是类数组，就不能直接用原生js的语法

```js
$('#box').style.backgroundColor = 'red';
```

上面写法是错的，$()选出的是jQuery对象，

如果想使用元素方法，可以加[0]，将jQuery对象转成元素对象

```js
$('#box')[0].style.backgroundColor = 'red';
```



#### 2.  使用方法（引号问题）

$(‘选择器’)里面的引号不能丢，在jQuery中只有是以下几个不用加引号，其他全部需要

不需要加引号的选择

```js
$(this)
$(document)
$(window)
```



#### 3. 选择器问题

jQuery支持的CSS2.1所有选择器,也支持部分CSS3的选择器

```js
$('div')
$('.box')
$('.box li')
$('*')
```



#### 4. 文档加载

```js
// 文档加载完毕后执行
$(document).ready(function(){
    
})
// 简写方案
$(function(){
    console.log($('div'))
})
```



### 三. 筛选器

这个是jQuery 的发明 和js没有关系

写在引号里面,:当成筛选的功能符

以下都是序号，筛选器,



#### 1.  选择所有的p标签

```js
$('p');
$('p').css('background-color','red');
```



#### 2. 选择第一个p标签

```js
$('p:first');
$('p:first').css('background-color','red');
```



#### 3. 最后一个p标签

```js
$('p:last');
$('p:last').css('background-color','red');
```



#### 4. 任意一个p标签，选用下标

: eq(num)； num是要筛选出来第几个p的下标，从0开始

```js
$('p:eq(2)');
$('p:eq(2)').css('background-color','red');
```



#### 5. 选中某一个范围的p标签

:lt(num)；  选中下标小于num的所有标签

```js
$('p:lt(2)');
$('p:lt(2)').css('background-color','red');
```

:gt(num);  选中下标大于num的所有标签

```js
$('p:gt(2)');
$('p:gt(2)').css('background-color','red');
```

等于就是:eq()



#### 6. 获取奇偶数

:odd  获取下标为奇数的标签

```js
$('p:odd');
$('p:odd').css('background-color','red');
```

:even  获取下标为偶数的标签

```js
$('p:even');
$('p:even').css('background-color','red');
```

> 注意哦
>
> 下标是从0开始的，也就是说下标为偶数的标签其实是我们看起来的奇数标签



#### 7. 特别的过滤器的方法

##### 7.1  eq可以单独的提炼为方法，可以连续打点调用

```js
$('p').eq(2);
$('p').eq(2).css('background-color','red');
```

提炼出来的好处是，可以使用变量

```js
var num = 2;
$('p').eq(num).css('background-color','red');

// 不提炼出来的方法，只用用字符串拼接了
$('p:gt('+ num +')').css('background-color','red');
```



还可以组合使用

```js
$('p:even').eq(2).css('background-color','red');
```



##### 7.2 first()  last()  筛选第一个最后一个元素的方法

```js
$('div').first().css('background-color','red')
```

```js
$('div').last().css('background-color','red')
```



##### 7.3 not() 排除那些元素

```js
$('div').not('.ta').css('background-color','red')
```



##### 7.4 hasClass() 判断是否具有某个类名

```js
$('div').click(function(){
    console.log($(this).hasClass('ta'))
})
```



#### 8. is()

判断是不是返回true,或false

判断点击的这个p标签是不是有这个类叫做t

```js
$('p').click(function(){
    alert($(this).is('.t'))
})
```





### 四. 序与迭代

#### 1. 序号 eq()

按照选择器选中的元素,然后在通过序号挑选

```html
<div class="box1">
    <p></p>
    <p></p>
    <p></p>
</div>
<div class="box2">
	<p></p>
    <p></p>
    <p></p>
</div>

<script>
	$('.box2 p').eq(1);
    $('p').eq(1);
    
    //按照需要的规则,上面两种写法选中并不是同一个p标签
</script>
```

也就是说 $()函数将返回一个对象队列,用eq来精确选择这个序号的某个元素.



#### 2. index() 方法

返回这个元素在亲兄弟中的排名,无视选择器怎么选.

```html
<div class="box1">
    <p></p>
    <p></p>
    <p></p>
</div>
<div class="box2">
	<p></p>
    <p></p>
    <p></p>
</div>

<script>
    $('p').click(function(){
        alert($(this).index());
    })
</script>
```

$(this).index()是一个很常见的写法,表示触发这个事件的元素,在亲兄弟中的排名.



利用index()方法写对应

```js
// 事件绑定在box1里面的p
$('.box1 p').click(function(){
    // 改变的是box2里面的对应的p
    $('.box2 p').eq($(this).index()).css("background-color","red");
})
```



#### 3. each() 方法   迭代

表示遍历节点,也叫作迭代符合条件的节点

```js
$('p').each(function(i){
    // i 为遍历的下标 0,1,2...
    $(this).css('width',50*i)
})
```



#### 4. size() 方法 和 length 属性

size() 方法和 length 属性是一样的的, 获取jQuery对象中元素的个数.

这两个数字永远相同

```js
$('p').length;
$('p').size();
```



#### 5. get() 方法

get()方法和eq()方法基本一致,都仰赖$()的序列,eq()返回的是jQuery对象.

而get()方法返回原生的JS元素,类似于直接jQuery元素加[0]

```js
$('p').get(0).innerHTML = '';
$('p')[0].innerHTML = '';
```



### 五. CSS 方法

CSS方法可以读取样式，可以设置样式

#### 1.  读取样式值

读取样式，可以读取计算后的样式，写一个参数，为获取值的属性

参数为属性字符串，必须加引号

读取的值有单位

```js
$('p:first').css('background-color');
```



#### 2. 设置一个属性值

如果只设置一个属性值，需要穿两个参数，

第一个参数为需要设置值的属性

第二个参数为需要设置的值，如果为数值，不需要加单位，

```js
$('p:first').css('background-color'，'blue');
$('p:first').css('width',200);
```



#### 3. 同时设置多个属性值

如果想要同时设置多个属性值，可以写成JSON

```js
$('p:lt(3)').css({
    'width': 100,
    'height': 100,
    'background-color': 'blue'
});
```

当然你也可以写多条单独设置样式



#### 4. 设置的属性可以多样

设置的属性不仅可以为改变后的值，还可以设置+= 的值，就是在原有的基础上加多少像素

```js
$('p:eq(3)').css('width','+=20px');
$('p:eq(3)').css('width','+=20');
```

 以上两种写法一样



#### 5. 设置获取自定义属性attr()

##### 5.1  获取自定义属性

```js
$('div').attr('data-title');
```

##### 5.2 设置自定义属性

```js
$('div').attr('data-title','wuwei');
```



#### 6. 关于class类名

##### 6.1 添加类名 addClass()

```js
$('div').addClass('title')
```

##### 6.2 删除类名 removeClass()

```js
$('div').removeClass('title')
```

##### 6.3 切换类名 toggleClass()

```js
$('div').click(function(){
    $('div').toggleClass('title')
})
```



#### 7. 关于节点值

##### 7.1 html()

就是innerHTML,只设置筛选出来的第一个符合条件的元素.

```js
$('div').html();  // 获取值
$('div').html('<h2>this is h2</h2>'); // 设置值
```

##### 7.2 text()

就是innerText

```js
$('div').text();  // 获取值
$('div').text('<h2>this is h2</h2>'); // 设置值
```



### 六. 节点关系

#### 1.  children()

选中所有的子元素

表示选取亲儿子,不选择后代,选择所有的子元素

```js
$('#box').children()
```

选择所有子元素中的div元素

```js
$('#box').children('div')
```

还可以添加筛选器

```js
$('#box').children('div:odd')
```

odd:奇数   even:偶数

#### 2. find()

查询所有的后代选择器

返回后代所有元素的列表

```js
$('#box').find('p')
```

注意,和children()方法不一样,find()方法里面,必须写参数,表示后代的谁,

find就是寻找的意思,就是你找后代里的谁



#### 3. parent()

找父元素,任何一个元素只有一个父元素,

```js
$('p').parent()
```



#### 4. parents()

找所有的祖先元素,可以传参数,找哪一个祖先

```js
$('p').parents('div')
```



#### 5. siblings() 

找所有的亲兄弟元素节点

```js
$('p').siblings()
```

可以加选择器

```js
$('p').siblings('div')
```



#### 6. prev()  next() prevAll() nextAll()

前一个兄弟,后一个兄弟,前所有的兄弟,后所有的兄弟元素节点



#### 7.  offsetParent()

查找定位父级

```js
$('p').offsetParent('div')
```





### 七.  节点操作

#### 1. append()  //相当于原生JS中的appendChild()

在父元素后面添加子节点

父节点.append(子节点)

```js
$('#box').append('<p>好的</p>')
```

如果插入的是原来就有的节点,则是移动节点.

##### 1.2 $()可以将一个普通的节点字符串转成jQuery对象

```js
var $p = $('<p>this is p</p>');

// 不能这么写 ,下面的写法是选择文档中的p标签了
$('p')
```



##### 1.3  创建节点

```js
$('<p></p>')
```

$()不仅仅能选择节点也能创建节点



#### 2. appendTo()

将一个jQuery元素添加到另外一个元素中

和append()方法是相反的,被动形式,追加于

```js
$('<p>好的</p>').append($('#box'))
```

将子元素插入到一个父节点中去.

#### 3. prepend()

在父元素最前面添加节点元素

```js
$('#box').prepend('<p>好的</p>')
//在#box所有 子节点的最前面添加<p>好的</p>
```



#### 4. prependTo()

prependTo()被动形式插入到父元素节点最前面

```js
$('<p>好的</p>').prependTo($('#box'))
```

将$('<p>好的</p>') 插入到$('#box')的所有子节点的最前面

#### 5. after()



在选中的元素后面插入一个兄弟元素节点

所有p标签后面插入一个h3兄弟元素

```js
$('p').after('<h3>我是h3标签</h3>')
//在$('p')的后面插入一个兄弟元素'<h3>我是h3标签</h3>'
```



#### 6. before()

在前面插入一个兄弟元素节点

```js
$('p').before('<h3>我是h3标签</h3>')
//在$('p')的前面插入一个兄弟元素'<h3>我是h3标签</h3>'
```



#### 7.insertBefore()

不同于原生的方法,在兄弟节点前插入新的节点

```js
$('<p>好的</p>').insertBefore($('p')[2])

//$('<p>好的</p>')插入到$('p')[2]之前
//相当于与原生js中 父节点.insetbefore(new,old)
```



#### 8. wrap()

给选中的元素外边添加一个包裹元素

```js
$('div').wrap('<p></p>')
//在$('div')外面包装一个p标签
```



#### 9. wrapAll()

将所有的选中的元素外套一个元素

```js
$('div').wrapAll('<p></p>')

//将所有的div用一个p标签包裹起来,千万不要跨层级使用,否则会剪切不同层级的元素到一起
```



#### 10. replaceWith()

将选中的元素替换掉,元素节点替换

```js
$('div').replaceWith('<p></p>') //p标签替换div标签
```



#### 11. empty()

清空元素里面的内容

```js
$('div').empty()  //清空子节点
```



#### 12. remove()

删除节点,删除页面上所有的p标签

```js
$('p').remove()
//返回值为删除的节点
```



#### 13. clone

节点克隆

参数为布尔值,不传参数默认为false

```js
$('div').clone();
$('div').clone(true);
// true表示要克隆div元素身上的事件 
//不论是true还是flase都复制后代节点
//复制的是jquery的事件

```

 原生js中parent.cloneNode(true)传入true复制子代节点,false不复制子代节点.事件都不会克隆.

### 八.  事件监听

#### 1. 通过事件名绑定事件

在jQuery里面,就连点击事件都变成回调函数了,

事件名一律不加on

```js
$('.box1').click(function(){
    // 这里就是点击box1要做的事情
})
```

例子:

```js
// 鼠标进入
$('p').mouseenter(function(){
    $(this).css('background-color','blue');
})

// 鼠标离开
$('p').mouseleave(function(){
    $(this).css('background-color','red');
})
```



#### 2. 通过on绑定事件

```js
$('.box1').on('click',function(){
    // 这里就是点击box1要做的事情
})
```



#### 3. 解除事件绑定off

```js
$('.box1').off('click')
```



#### 4. 只绑定一次事件one()

```js
$('.box1').one('click',function(){
    // 这里就是点击box1要做的事情
})
```



#### 5. 移入移出事件

```js
$('.box1').hover(function(){
    // 鼠标移入触发
    console.log(1);
},function(){
    // 鼠标移出触发
    console.log(2);
})
```



#### 6. 事件对象事件源

e.target 获取的是元素的DOM元素

```js
$(document).click(function(e){
    console.log(e.target);   // 原生DOM元素
})
```



### 九. animate 方法

动画方法 animate

#### 1.  animate 方法的使用

第一个参数：终点JSON    要操作的属性

第二个参数： 动画运行的时间，毫秒

```js
 $('p:eq(3)').animate({'margin-top': 300},2000);
```



#### 2.  动画运行完的回调函数

第三个参数为动画运行完后的参数

背景颜色是不能在动画里渐变的,只有在回调里完成,如果想让颜色慢慢渐变需要使用css3技术.

```js
$('p:eq(3)').animate({'margin-top': 300},2000，function(){
    // alert('运行完成')；
    $(this).css('background-color','red')；
});
```

jQuery动画默认不是匀速的 ，是easeInOut



#### 3.  动画排队

##### 3.1. 同一个元素的不同动画会排队

```js
 $('p:eq(3)').animate({'top': 300},2000);
 $('p:eq(3)').animate({'left': 300},2000);
```

jQuery的动画和我们封装的不一样，不是斜着走的，而是先向下运动，结束后在向右移动

因为jQuery默认有一个处理机制，叫做 动画排队

动画排队：

当一个元素接收到了两个animate命令后，后面的animate会排队

所以上面的动画，先竖着跑在横着跑，总动画时长为4000毫秒



如果想斜着跑就写在一个animate里面

```js
 $('p:eq(3)').animate({'top': 300,'left': 300},2000);
```

 我们自己封装的动画方法不如它的原因就在这里，没有动画排队



##### 3.2. 不同的元素动画不会排队 是同时的

这里的p选择的是多个元素，不排队同时运行动画

```js
 $('p').animate({'top': 300},2000);
```



### 十. 动画相关的方法

#### 1. 内置的show(),hide(),toggle()方法

show()显示,hide()隐藏,toggle()切换

使用方法:

```js
show([time[,callback]])

```

参数都是可选的



##### 1.1 没有参数的时候

```js
$('div').show();  // 让div元素显示
$('div').hide();  // 让div元素隐藏,添加display:none
$('div').toggle();// 切换显示状态,自行带有判断,如果显示,则隐藏,如果隐藏则显示
```



##### 1.2 如果有数值参数,将变为动画,第一个参数为运动速度或时间

速度单词,fast normal, slow ,参数为字符串

如果传入的为时间,则单位为毫秒数

让元素在显示与影藏之间动画运动1s,还有透明度变换,

```js
$('div').show(1000);  // 从左上角徐徐展开
$('div').hide(1000);  // 徐徐缩小到左上角,运动完成添加display:none
$('div').toggle(1000);// 切换显示状态,自行带有判断,如果显示,则隐藏,如果隐藏则显示
```



##### 1.3  第二个参数为运动完成后的回调

```js
$('input:eq(0)').click(function(){
    $('div').show(1000,function(){
        $(this).css('background','red');  // 动画运行完执行的回调函数
    });
})
```





#### 2. slideDown(),slideUp(),slideToggle()

滑动,卷帘运动,这个动画改变的就只有元素的高度

使用方法:

```js
slideDown([time[,fn]])
```

参数都是可选的



##### 2.1 不加参数的情况

slideDown()方法的动画机理

> 一个display:none;的元素,瞬间显示,瞬间高度变为0,然后jQuery自己捕捉原有的height作为动画的重点.
>
> 最好不要用行内元素做动画
>
> 等价于:
>
> $('div').show();                                                  //  瞬间显示
>
> var oldHeight = $('div').css('height');             //  记住原有高度
>
> $('div').css('height':0);					    //   将高度设置为0
>
> $('div').animate({'height':oldHeight},1000);  // 执行高度从0到原有高度的动画

```js
$('div').slideDown();  // 下滑展开
$('div').slideUp();    // 上滑收回
$('div').slideToggle();// 滑动切换
```



```js
$('input:eq(0)').click(function(){
    $('div').slideDown();
})
$('input:eq(1)').click(function(){
    $('div').slideUp();
})
$('input:eq(2)').click(function(){
    $('div').slideToggle();
})
```



##### 2.2 第一个参数为运动速度或时间

速度单词,fast normal, slow ,参数为字符串

如果传入的为时间,则单位为毫秒数

```js
$('div').slideDown(1000);
```



##### 2.3 第二个参数为回调函数

```js
$('div').slideDown(function(){
    $(this).css('background','red')
});
```



##### 2.4 结合上面两种方法自动执行

```js
$('div').show(1000,function(){
    $(this).slideUp(1000,function(){
        $(this).slideDown(1000,function(){
            $(this).hide(1000)
        })
    })
})
```





#### 3. fadeIn(),fadeOut(),fadeTo(),fadeToggle();

淡入淡出的一系列方法

如果元素有opacity属性,一定要理解,你所设置的opacity值为,淡入的终点值,如果你设置为0,那么元素将永远淡入不了;

fadeIn()  动画机理:

> 一个display:none的元素,瞬间可见,然后透明度瞬间变为opacity:0,然后自己的opacity开始变换,如果自己没有设置opacity,就变为1



##### 3.1  不加参数执行淡入淡出

```js
$('div').fadeIn();      // 淡入
$('div').fadeOut();     // 淡出 
$('div').fadeTo();      // 淡入到哪个数字,不传参数没效果,最好是时间和最终的opacity值一样设置.
						// 这个比较特殊,如果起点至比较大,就是淡出,起点值小,就是淡入
$('div').fadeToggle();  // 淡入淡出切换
```



##### 3.2  第一个参数为速度或时间

```js
$('div').fadeIn(1000);      // 淡入过程1000完成
$('div').fadeOut(1000);     // 淡出 
$('div').fadeTo(1000);      // 第一参数为过渡时间,第二个但是才是淡入到哪个数字
$('div').fadeToggle(100);  // 淡入淡出切换
```



##### 3.3  第二个参数是回调函数

```js
$('div').fadeIn(1000,function(){
    
});      
```

比较特殊的是fadeTo()

fadeTo() 方法第二个参数是,透明度变换的终点值,第三个参数才是回调函数

```js
$('div').fadeTo(1000,0.6,function(){
    
});    
```



#### 4. stop() 停止动画

关于动画停止一共有四种,不同的参数情况不同

```js
stop();//只停止当前动画,不停止动画对列
stop(true);//停止当前动画,并停止动画对列
stop(true,true);//事件触发之时瞬间停止当前动画并到达原预定位置,同时停                   止动画队列.
stop(false,true);
##### 总结会发现,

第一个参数为是否清除动画队列,true,清除动画队列,false不清除动画队列

第二个参数为,停止当前animate动画,停止后动画的位置,true为,瞬间结束动画,动画停止后位置为当前动画的终点,false,停止动画值,元素停留在停止动画的位置
```



##### 4.1 stop()没有参数的情况

stop ()停止动画如果没有参数,则表示停止当前的animate动画,但是不清除动画队列,立即执行后面的animate动画

```js
$('div').stop();
```



##### 4.2 stop(true)

停止当前animate动画,并且清除动画队列,盒子此时留在停止动画时的位置

```js
$('div').stop(true);
```



##### 4.3 stop(true,true)

停止当前animate动画,盒子瞬间停当前animate动画的终点位置,并且清除动画队列,

可以理解为瞬间执行完当前动画,并且清除动画队列

```js
$('div').stop(true,true);
```



##### 4.4 stop(false,true)

瞬间完成当前的animate动画,但是不会清除动画队列,并且执行后面的动画,

```js
$('div').stop(false,true);

```



有人可能会有疑问,为什么没有stop(true,false);

 其实stop(true),就是stop(true,false);后面的false可以省略,

现在就明白了,stop()就是stop(false,false);



##### 总结会发现,

第一个参数为是否清除动画队列,true,清除动画队列,false不清除动画队列

第二个参数为,停止当前animate动画,停止后动画的位置,true为,瞬间结束动画,动画停止后位置为当前动画的终点,false,停止动画值,元素停留在停止动画的位置



动画案例:

```html
<style type="text/css">
    div {
        position:absolute;
        top:100px;
        left:100px;
        width: 100px;
        height: 100px;
        background-color: skyblue;
    }

</style>

<input type="button" value="stop()">
<input type="button" value="stop(true)">
<input type="button" value="stop(true,true)">
<input type="button" value="stop(false,true)">

<div></div>

<script type="text/javascript" src="jQuery.js"></script>
<script type="text/javascript">
    // 添加四个动画
    $('div').animate({'left':800},2000);
    $('div').animate({'top':300},2000);
    $('div').animate({'left':100},2000);
    $('div').animate({'top':100},2000);


    $('input:eq(0)').click(function(){
        $('div').stop();
    })
    $('input:eq(1)').click(function(){
        $('div').stop(true);
    })
    $('input:eq(2)').click(function(){
        $('div').stop(true,true);
    })
    $('input:eq(3)').click(function(){
        $('div').stop(false,true);
    })
</script>

```



##### 4.5 stop()可以用来防止动画积累

每次点击都会在动画队列里添加一个动画,动画会排队

```html
<script type="text/javascript">
    $('input:eq(0)').click(function(){
        $('div').animate({'left':100},1000);
    })
    $('input:eq(1)').click(function(){
       $('div').animate({'left':900},1000);
    })
</script>

```

就像定时器,设表先关

用动画先关闭前面的动画

可以连续打点的调用,先清除动画队列,再行新的动画

```js
$('input:eq(0)').click(function(){
    $('div').stop().animate({'left':100},1000);
})
$('input:eq(1)').click(function(){
    $('div').stop().animate({'left':900},1000);
})
```





#### 5. finish()

瞬间完成所有动画队列

```js
$('input:eq(4)').click(function(){
    $('div').finish();
})
```



#### 6. deley()

延迟,可以使用连续打点,必须放在运动语句之前

```js
$('div').delay(1000).animate({'left':500},1000);
$('div').delay(1000).slideUp();

```

注意hide()不加参数,就不是动画,是瞬间完成的,加参数哪怕数字1也是动画

```js
$('div').delay(1000).hide(3000);
```



#### 7. is(":animted")

判断一个元素是否在运动中,可以防止动画积累

```js
$('div:eq(3)').animate({'width':900},8000);
$('div').click(function(){
    alert($(this).is(':animated'))
});

```

