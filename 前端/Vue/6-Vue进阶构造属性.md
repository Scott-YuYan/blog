#### 1.Directives

除了之前学习过的v-if、v-for、v-show、v-html等，我们还可以通过directives自定义指令：

* 1.声明一个全局指令：

```
Vue.directive('x',{
   inserted:function (el) {
       el.addEventListener('click',()=>{
           console.log('x');
       })
   }
});
```
可在任一地方使用v-x指令。

* 2.声明一个局部指令

```
        directives:{
            x:{
                inserted:function (el) {
                    el.addEventListener('click',()=>{
                        console.log("inner x");
                    })
                }
            }
        }
```
指令则只能在该组件中使用。<br>

* 3.directivesOptions的五个函数属性:
<ul>
    <li>bind:(el,info,vnode,oldVnode)-类似created</li>
    <li>inserted(参数同上)-类似mounted</li>
    <li>update(参数同上)-类似updated</li>
    <li>componentUpdated(参数同上)-指令所在组件的 VNode 及其子 VNode 全部更新后调用。(用的不多)</li>
    <li>unbind(参数同上)-类似destroyed</li>
</ul>

举例：
```
<button id="add" v-click2:click="add">+2 - MyClick</button>
methods:{
   add(){
        this.n+=2;
    }
}

directives:{
    click2: {
        inserted: function (el, binding) {
            el.addEventListener(binding.arg, binding.value);
            }
        },
        //习惯性在元素消亡时移除时间监听
        unbind(el,binding){
            el.removeEventListener(binding.arg, binding.value);
        }
    }
```
当然，也可使用钩子函数绑定：
```
        mounted() {
            this.$el.querySelector('#add').addEventListener('click',()=>{this.n+=2})
        }

```

* 总结指令的作用

主要用于DOM操作：
<ul>
<li>Vue实例/组件用于数据绑定、事件监听、DOM更新。</li>
<li>Vue指令主要目的就是原生DOM操作。</li>
</ul>

减少重复：

<ul>
<li>如果某个DOM操作你经常使用，就可以封装为指令</li>
<li>如果某个DOM操作比较复杂，也可以封装为指令</li>
</ul>

#### 2.mixins-混入(就是复制)

##### 2.1 减少重复
* directives是减少DOM重复的操作
* mixins是减少data、methods、钩子的重复

实例：给每个组件增加一个存活时间的计算<br>

首先当然可以给每个组件增加一个created()及beforeDestroy()钩子，但是为了避免重复，可以用mixins<br>,
code:<br>
新建log.js
```
const log = {
    data() {
        return {
            name: undefined,
            time: undefined
        }
    },
    created() {
        if (!this.name) {
            throw new Error("need name!");
        }
        this.time = new Date();
        console.log(`${this.name} 创建了 `)
    },
    beforeDestroy() {
        const now = new Date();
        console.log(`${this.name} 死亡了，存活时间 ${now - this.time}`)
    }
};
export default log;
```
在其他组件中使用mixins引入：
Child1.vue:
```
export default {
    data() {
        return {
            name: "child1"
        }
    },
    template: `
        <div>
            {{this.name}}
        </div>
    `,
    mixins:[log]
};
```
注意这里的data与log.js中的data会智能合并<br>
在组件外面控制是否显示：
main.js:    
```
<div v-if="child1Visible">
    <Child1/>
</div>
<button @click="child1Visible = !child1Visible">显示Child1/隐藏</button>
```

当如，如果需要给所有的组件增加mixins，
可以使用全局的`Vue.mixins` <a href="https://cn.vuejs.org/v2/api/#Vue-mixin">Vue-mixins</a>


#### 3.Extends 继承、拓展
与前面同样的情况，但是不想每次都写mixins,举例：<br>
可用extend自定义Vue
MyVue.js
```
const Vue = window.Vue;
import log from "./mixins/log";
const MyVue = Vue.extend({
    mixins:[log]
});
export default MyVue;
```
Child1.vue:
```
<script>
    import MyVue from "../MyVue";

    export default {
        extends: MyVue,
        template: `
            <div>{{name}}</div>`,
        data() {
            return {
                name: "child1"
            }
        }
    }
</script>
```
extends是比mixins更抽象的一种封装

#### 4.provide-提供和inject-注入
提供一个组件，使得任何引用该组件的组件，都可以修改主题颜色：

Provide.vue
```
<template>
    <div>
        <div :class="`app theme-${themeColor}`">
            依赖和注入
        </div>
        <ChangeThemButton/>
    </div>


</template>

<script>
    import ChangeThemButton from "./ChangeThemButton";

    export default {
        data() {
            return {
                themeColor: "red"
            }
        },
        // 通过provide将changeThemColor暴露给ChangeThemButton组件
        provide() {
            return {
                //由于传入的是对象的引用，所以可以直接将方法或者对象的引用传给其他组件
                changeThemColor: this.changeThemColor
            }
        },
        methods: {
            changeThemColor() {
                if (this.themeColor === 'blue') {
                    this.themeColor = 'red'
                } else {
                    this.themeColor = 'blue'
                }
            }
        },
        components: {
            ChangeThemButton
        }
    }
</script>


<style>
    .app {
        font-size: 12px;
    }

    .theme-blue {
        color: blue;
    }

    .theme-red {
        color: red;
    }
</style>
```

ChangeThemeButton.vue:

```
<template>
    <div>
        <button @click="change">切换主题</button>
    </div>
</template>

<script>
    export default {
        // 通过inject 接收来自其他组件提供的属性的引用
        inject: ['changeThemColor'],
        methods: {
            change() {
                this.changeThemColor()
            }
        }
    }

</script>
```

