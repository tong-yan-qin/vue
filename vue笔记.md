### VUE视频笔记

#### vue基础

##### 一秒看懂vue代码

```
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



