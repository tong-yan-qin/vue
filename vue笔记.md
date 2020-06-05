### VUE视频笔记

#### vue基础

##### 一秒看懂vue代码

```html
 <div id="app">
        <div>{{message}}</div>
    </div> 
    <script>
        let app = new Vue({
            el: "#app",
            data: {
                message: "hellow vue",
            }
        })
    </script>
```

#####  mustache语法

{{}}也称双括号语法，可以填充data数据，以及对数据的拼接，简单的运算等。

##### 简单的指令

v-once 指令：只将第一简单的指令
v-once 指令：只将第一次的数据进行显示，之后不再随data数据改变
v-html 指令： 对数据进行html解析
v-text 指令： 显示text，会覆盖原本html标签体数据
v-pre 指令： 把数据原封不动的显示出来，不进行任何解析
v-cloak指令：可以配合css使用，作用是当渲染vue后，该指令自动被删除 （斗篷）

##### v-bind 绑定属性

mustache语法只能显示内容位置的数据，无法在属性里面使用该语法，所以就要使用v-bind对标签的属性进行绑定

```
< img v-bind:src="imgURL">
<a v-bind:href="www.baidu.com">百度一下</ a>
```

v-bind 经常被使用到，所以有个简写，将v-bind省略

```
< img :src="imgURL">
<a :href="www.baidu.com">百度一下</ a>
```


这样就可以实现属性的绑定，而class内的属性需要经常变化，因而就要使用v-bind双向绑定了

```
<h2 v-bind:class="{active: isActive, line: isLine}">{{message}}</h>
```

该语法被单大括号包围，所以也叫对象语法，格式为{类名：boolean，类名：boolean...}
既然该语法内部是一个对象，所以可以用使用调用函数返回该对象填入class内部的形式实现

```
<body>
    <div id="app">
        <div :class="getCalss()">演示字体</div>
    </div>
    <script src="../lib/vue/vue.js"></script>
    <script>
        const app = new Vue({
            el: "#app",
            data: {
                isActive: false
            },
            methods: {
                getCalss: function(){
                    return {active: this.isActive}
                }
            }
        })
    </script>
</body>
```

v-bind不仅可以是对象，还可以是数组，使用较少，内容写死，这里不说了

##### v-bind 动态绑定style

需要注意的是值(key)需要加上单引号，否则会被当作变量，渲染后单引号自动去除。

```
<h2 :stlye ="{fontSize: '20px'}">{{message}}</h2>
```

数组形式的style是把多个style对象放在一起。不想多说。

##### computed 计算属性 get 和 set

computed的写法通常是这样的

```html
<h2>{{funnName}}</h2>
```

```javascript
computed: {
    fullName: function(){
        return this.data1 + " " + this.data2
    }
}
```

而fullName可以直接当作mustache语法的参数使用。乍一看，fullName是一个函数，作为语法的参数为何不用加小括号（）来调用一下呢。不需要的，计算属性其本质就是一个普通的属性。fullName是一个对象,里面有 get() 和 set() 方法。由于计算属性一般只用到 get() 方法作为只读属性，所以上面的写法是一个简写。set() 方法如果要实现，其有一个参数 newValue 作为传入设置的参数。

##### 计算属性和methods对比

methods 和 computed 的简写写法一样，那使用computed与methods的区别是什么呢

- computed 作为本质是属性的它，当多次被调用，而值没有发生改变的时候，它的方法体内容只会被执行一次，它的值是被缓存起来的。而methods 调用几次就被执行几次。

##### ES6语法补充

###### let/var/const

let 可以看作是更完美的 var，let 有块级作用域。也就是var生命不受大括号限制。if/for等都是块级作用域。

const 与 let 的区别是，const是一个常量，为了使代码更加规范，优先考虑使用const。const使用注意点：

- 一旦给const变量赋值后不能修改
- 声明的时候必须进行赋值
- 如果变量是一个对象，这时内部的属性是可以修改的。

###### for循环

```
//取到下标
for (let i in books){
}
//直接取元素
for （let book of books）{
}
```

###### 对象字面量增强写法

普通写法

```
const obj = {
	name: 'why',
	age: 18,
	run: function(){
		
	}
}
```

增强写法

```
const name = why;
const age = 18;
const obj = {name, age}
```

```
const obj = {
	run() {
		
	}
}
```

若已经有值，可以直接写入，不用赋值操作。会自动给对象加个属性名和变量一样的属性。

##### v-on事件监听

###### v-on的基本写法

```html
<button v-on:click="increment">+</button>
<button v-on:click="increment">-</button>
```

```
methods:{
	increment(){
		this.counter++
	},
	decrement(){
		this.counter--
	}
}
```

v-bind的简写是将v-bind省略。而v-on的简写是将v-on:替换为@

```html
<button @click="increment"></button>
```

###### v-on参数问题

事件调用方法的时候可以不加小括号也可以，这时如果调用的方法需要一个参数的话，vue会默认将浏览器生成的event事件对象传入。

当需要event对象，同时又需要其他参数的时候。我们可以这样传入

```html
<button @click="clickBtn('abc',$event)">按钮</button>
```

调用时手动取到event对象：$event。

###### v-on修饰符

```html
<div @click="divClck">
    <button @click.stop="btnClick">按钮</button>
</div>
```

- .stop：阻止冒泡，调用event.stopPropagation()
- .prevent：阻止默认事件，调用event.preVentDefault()
- .enter: 监听某个键盘的点击。enter为回车
- .native:监听组件根元的原生事件
- .once：只触发一次回调

##### v-if-else条件判断

```html
<h2 v-if="isShow">
	{{message}}
</h2>
<h2 v-else-if="isShow1">
    {{message1}}
</h2>
<h2 v-else>
    {{message2}}
</h2>
```

isShow为条件变量。和else配合使用.。

##### v-show使用以及与v-if的区别

v-show与if...else使用方法以及效果是相同的。有区别的是v-if-else当条件为false的时候，被包含的元素不会存在dom中。当为true的时候会重新创建一个。而v-show就是给元素添加一个行内样式：display:none/block

使用选择的时候要看切换的频率。频率高的话就选择v-show，不过大多时候使用v-if

##### v-for循环遍历

###### 数组

```html
<ul>
    <li v-for="item in names">{{item}}</li>
</ul>
```

```html
<ul>
    <li v-for="(item,index) in names">{{index+1}}.{{item}}</li>
</ul>
```

```
data: {
	names: ['why','zhangcheng','hks']
}
```

###### 对象

```html
<ul>
    <li v-for="item in obj">{{item}}</li>
</ul>
```

```html
<ul>
    <li v-for="(value,key) in obj">{{key}}-{{value}}</li>
</ul>
```

```html
<ul>
    <li v-for="(value,key,index) in obj">{{key}}-{{value}}</li>
</ul>
```

```javascript
data: {
	bjj: {
		name: 'hh',
		age: '27',
		gender: 'nan'
	}
}
```

在遍历对象的过程中，如果只是获取一个值，那么获取到的是value。可以获取三个值，键值下标

##### 数组响应式方法

```
push()：添加到最后，多个
pop()：删除最后一个
shift()：删除第一个
unshift()：添加到第一个
splice()：删除、插入、替换元素[起始,个数,新元素]
sort()：排序
revert()：反转
set()：[目标数组,索引,新元素]
```

数组下标索引的方式不是响应式的，所以需要用到以上的方法替换

##### 过滤器使用

与method、computed一样，过滤器使用filters表示，使用如下

```html
<td>{{item.price | showPrice}}</td>
```

```javascript
filters: {
	showPrice(price){
		return '￥' + price.toFixed(2)
	}
}
```

##### 高阶函数

编程范式：命令式编程、声明式编程、面向对象编程、函数式编程

###### filter

该函数必须返回一个布尔值，当为true时会加入到新数组里面，如果为false，就会被过滤掉

```
let newNums = nums.filter(function (n){
	n<100;
})
```

###### map

遍历并返回一个新的元素加入新的数组

```
let new2Num = newNums.map(function(n) {
	return n*2;
})
```

###### reduce

对数组里面的所有内容进行汇总

```javascript
/**
* @previousValue 前一个值
* @init 初始化值
*/
let new2Nums = new2Num.reduce(function(preValue, n){
	return preValue + n
},0)
```

##### 表单绑定v-model使用

用户改变表单的值，data的值也会被改变

```html
<input type="text" v-model="message"> {{message}}
```

##### v-model修饰符

- .lazy：懒加载，失去焦点才会绑定
- .number：绑定的值为数字
- .trim

#### 组件化

##### 基本使用

创建组件构造器对象

```javascript
cibst cpnC = Vue.extend({
	template: `
		<div>
			<h2>标题</h2>
			<p>内容</p>
		</div>`	
})
```

注册组件

```
Vue.component("my-cpn",cpnC)
```

##### 局部组件

与data、methods一样

```
components:{
	cpn: cpnC
}
```

##### 父子组件

Vue.extend()方法传入的参数对象除了有template外还可以增加一个components进行注册组件，然后可以将组测的组件在父组件中使用。

```javascript
cibst cpnC1 = Vue.extend({
	template: `
		<div>
			<h2>标题1</h2>
			<p>内容1</p>
		</div>`	
})

const cpn = Vue.extend({
	template: `
		<div>
			<h2>标题</h2>
			<p>内容</p>
			<cpn1></cpn1>
		</div>`	,
    components: {
        cpn1: cpnC1
    }
})
```

子组件不能在父组件之外使用。

##### 注册组件语法糖

```
Vue.component('cpn1',{template:`xxx`})
```

```
components: {
	'cpn2': {
		template:`xxx`
	}
}
```

语法糖将extend使用对象代替

##### 组件注册与模板分离写法

```html
<script type="text/x-template" id="cpn">
	<模板内容>
</script>
```

```html
<template id="cpn"></template
```

```javascript
Vue.component('cpn',{
	template: '#cpn'
})
```

##### 父子组件通信

###### 父组件向子组件传数据

需要在注册组件的构造器中声明数据，有三种写法

```javascript
const cpn = {
    template: '#cpn', //模板
    //第一种:单纯的声明变量
    props: ['cmovies', 'cmessage'],
    
    //第二种:限制类型
	props:{
        cmovies: Array,
        cmessage: String
    }  
    
    //第三种:限制类型并提供默认值
    props: {
    	cmessage: {
    		type: String,
    		default: 'aaa',
    		requery: true //必须传值
		},
      	cmovies: {
            type: Array,
            defalut(){
                return []
            }
        }
	}   
}
```

然后再使用cpn标签的时候可以向子组件传输数据

```
<cpn :cmovies="movie" :cmessage="message"></cpn>
```

###### 驼峰标识

如果子组件使用的是驼峰命名，也就是包含大小写，那么在使用组件传值的时候，需要将子组件的变量名转成小写，并在大写转小写的字母前加横杠“-”，例如cMovies、cMessage如下

```
<cpn :c-movies="movie" :c-message="message"></cpn>
```

###### 子组件向父组件传输数据

子组件要向父组件传输数据要使用到 $emit() 注册事件，该事件可传输参数给父组件，具体写法如下

```
methods: {
	//发射事件、触发事件
	this.$emit('item-click',item)
}
```

父组件获取数据要通过使用的组件标签触发事件时调用父组件的方法传输

```
<cpn @item-click="cpnClick"></cpn>
```

父组件的方法

```
methods: {
	cpnClick(item){
		console.log(item)
	}
}
```

由此即可获取子组件的数据