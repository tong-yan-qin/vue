# vue  笔记

一秒看懂vue代码

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



##  mustache语法

{{}}也称双括号语法，可以填充data数据，以及对数据的拼接，简单的运算等。





## 简单的指令

v-once 指令：只将第一简单的指令
v-once 指令：只将第一次的数据进行显示，之后不再随data数据改变
v-html 指令： 对数据进行html解析
v-text 指令： 显示text，会覆盖原本html标签体数据
v-pre 指令： 把数据原封不动的显示出来，不进行任何解析
v-cloak指令：可以配合css使用，作用是当渲染vue后，该指令自动被删除 （斗篷）

绑定属性 v-bind
mustache语法只能显示内容位置的数据，无法在属性里面使用该语法，所以就要使用v-bing对标签的属性进行绑定
< img v-bind:src="imgURL">
<a v-bind:href="www.baidu.com">百度一下</ a>
v-bind 经常被使用到，所以有个简写，将v-bind省略
< img :src="imgURL">
<a :href="www.baidu.com">百度一下</ a>
这样就可以实现属性的绑定，而class内的属性需要经常变化，因而就要使用v-bind双向绑定了

```
<h2 v-bind:class="{active: isActive, line: isLine}">{{message}}</h>
该语法被单大括号包围，所以也叫对象语法，格式为{类名：boolean，类名：boolean...}
既然该语法内部是一个对象，所以可以用使用调用函数返回该对象填入class内部的形式实现
```

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

v-bind 动态绑定style
与绑定class不同的是其key为样式名，value为要设置的值，其实也是一样的。样式名的写法支持font-size写法的同时也支持fontSize的写法。
需要注意的是值(key)需要加上单引号，否则会被当作变量，渲染后单引号自动去除。

```
<h2 :stlye ="{fontSize: '20px'}">{{message}}</h2>
```

数组形式的style是把多个style对象放在一起。不想多说。