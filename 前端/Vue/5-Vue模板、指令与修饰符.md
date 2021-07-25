#### 1.三种模板的写法

##### 1.1 Vue完整版，写在HTML中
举例：
```
<div id="template2">
    {{n}}
    <button @click="add">+1</button>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script>
    new Vue({
        el: '#template2',
        data: {
            n: 0
        },
        methods: {
            add() {
                this.n += 1;
            }
        }
    });
</script>
```

####1.2 Vue完整版，写在选项中
举例：
```
<script>
export default {
  props: {
    msg: String,
    message:String
  },
  template:`
    <div class="hello" v-bind:title="message">
      <h1>{{ msg }}</h1>
    </div>
  `
}
</script>
```
注意：new Vue后 div#app会被替换掉

##### 1.3 非完整版的Vue,配合xxx.vue文件
```
<template>
    <div id="app">
        {{n}}
      <button id="add" @click="add($event)">+1</button>
    </div>
</template>
```
注意一点，template中的内容是XML语法，也就是说标签都需要闭合(必须写/)，另外，在XML中，空标签可以用`<div />`表示。

这种写法的引入代码：
```
import Xxx from '...'

new Vue({
  render:h=>h(Xxx)
}).$mount('...');

```

#### 2.Vue指令
* 语法 v-指令名：参数=值，如 `v-on:click=add`
* 如果值里没有特殊字符，则可以不加引号
* 有些指令没有参数和值，如v-pre
* 有些指令没有值，如v-on:click.prevent(prevent为修饰符，用于修饰指令)
##### 2.1 展示内容

* 表达式
```
{{object.a}} 表达式
{{n+1}} 可以写任何运算
{{ fn(n)}} 可以调用函数
<div v-text="{{n+1}}"> 数据绑定语法(不常用)
注意：如果值为undefined或null就不显示
```

* HTML
 在data中定义：` html:'<strong>123</strong>'`,那么可以在template中使用：
 `<div v-html="html"></div>`显示粗体的123
 
 * 不对元素进行编译，展示原始内容
 使用`v-pre`指令，例如：`<div v-pre>{{n}}</div>
 
 ##### 2.2 绑定属性
 
 * 绑定src
 `<img v-bind:src="x"` 这种写法可以简写为：`<img :src="x">`
 
 * 绑定对象
  `<div :style="{border:'1px solid red',height:'100'}></div>`,注意这里可以把'100px'写成100,但是这种写法只支持px,em、rem等还需要带单位
  
  ##### 2.3 绑定事件
  v-on 可以缩写为`@`
  * 1. `<button @click="add">+1</button>` 点击事件后，Vue会运行add()
  * 2. `<button @click="add(1)>+1</button>` 点击事件后，Vue会运行add(1)
  * 3. `<button @click="n+=1">+1</button>` 点击事件后，会运行代码
  
  得出结论：点击事件之后，发现是函数就调用函数，否则直接运行代码
  
  ##### 2.4 条件判断
  ```
<div v-if="">
</div>
<div v-else-if="">
</div>
<div v-else>
</div>
```

##### 2.5 循环

* for(value,key) in 对象或数组
数组：
```
<ul>
    <li v-for="(value,index) in users" :key="index">
        索引：{{index}}值：{{value.name}}
    </li>
</ul>
```

对象：
```
<ul>
    <li v-for="(value,name) in user" :key="name">
        属性名：{{name}},属性值：{{name}}
    </li>
</ul>
```

<strong>坑预警- `:key="index"`有bug</strong>

##### 2.6 显示、隐藏
 * v-show
  `<div v-show="n%2===0`>n是偶数</div>
  
  v-show为false的元素，不处于dom树中
 
##### 2.7 其他指令
* v-model

* v-slot

* v-cloak

* v-once

#### 3.修饰符
有些指令支持修饰符
* @click.stop="xxx" -表示阻止事件传播/冒泡
* @click.prevent="xxx" -表示阻止默认事件
* @click.stop.prevent='xxx' -表示两种意思

##### 3.1 所有的修饰符
* v-on 支持的有 .{keycode | keyAlias} / .stop / .prevent /.capture /.self /.once /.passive /.native
<br> 快捷键相关: .ctrl .alt .shift .meta .exact 
<br> 鼠标相关: .left .right .middle
<br>
keycode(keycode为符号对应的ASCⅡ码) 举例：

```
            <div>
                <input @keypress.13="enter"/>
            </div>

            entry(e) {
                console.log("回车");
                // if (e.keyCode === "13") {
                //     console.log("回车")
                // }
            }
```
keyAlias 按键别名enter,pageUp,pageDown等，<a href="https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6">按键修饰符</a>
* v-bind 支持的有 .prop .camel .sync
* v-model 支持的有 .lazy .number .trim (表单相关)

除与表单相关的三个外，常用的也就是`.stop .prevent .sync`

##### 3.2 .sync修饰符(很重要)
示例：由于子组件不能直接修改props中传过来的属性，所以可以通过this.$emit触发事件并传参，通知父组件修改：<br>
子组件:
```
<script>
    export default {
        template: `
        <div>
            <button @click="$emit('update:money',money-100)">消耗100</button>
        </div>
        `,
        props:{
            money:Number
        }
    }
</script>
```
父组件：
```
<script>
    import ChildComponent from "./ChildComponent";
    export default {
        data() {
            return {
                total: 10000
            }
        },
        template:`
            <div>
                {{total}}
                <hr>
                <ChildComponent :money="total" v-on:update:money = "total = $event"/>
            </div>
        `,
        components:{
            ChildComponent
        }
    }
</script>
```
 `<ChildComponent :money="total" v-on:update:money = "total = $event"/>` 是Vue中常见的场景(自己不直接修改，而是通过通知父组件修改属性)
 这种写法可以简写为：
 ` <ChildComponent :money.sync="total"/>`