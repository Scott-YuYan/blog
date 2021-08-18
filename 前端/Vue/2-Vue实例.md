#### 1.构造Vue的实例
构造代码：`const vm  = new Vue(options)`,也就是`[vm.__proto__ = Vue.protoType]` ,同时，需要注意：`Vue.__proto__ === Function.prototype`

说明： 

* vm对象封装了对视图的所有操作，包括数据读写、事件绑定、DOM更新
* vm的构造函数是Vue，按照ES6的说法，vm所属的类是Vue
* options是new Vue的参数，一般称之为选项或构造函数
#### 2.options中有那些可选项
* 数据：data、props、propsData、computed、methods、watch
* DOM：el、template、render、renderError
* 声明周期钩子:beforeCreate、created、beforeMount、mounted、beforeUpdate、updated、activated、deactivated、beforeDestroy、
destroyed、errorCapture。
* 资源：directives-指令、filters-过滤、components-组件
* 组合：parent、mixins-混入、extends-拓展、provide-提供、inject-注入、
* 其他：用的不多，看<a href="https://cn.vuejs.org/v2/api/#el">文档</a>

解释下生命周期钩子，首先生命周期，对象的生命周期大致分为：
```
let div = document.createElement('div')
// 这是div的create / construct过程
div.textConent = 'hi'
// 初始化state
document.body.appendChild(div)
// div的mount过程
div.textContent = 'h2'
//div的update过程
div.remove
// div的unmount过程
```
对应到Vue中
`createElement('div')/create->append('#app')/mounted->n:0=>1/updated->destroyed`,那么在这些生命周期中间，加上切入点，我们称这些点为钩子
可以理解为火车车厢连接部分的钩子。

其中propsData、render、errorCapture、parent不常用，需要用时再去看文档即可，filters看起来很有用，其实没什么用，尽量不用。

#### 3.入门属性
* el-挂载点
 与$mount有替换关系

例如：
```
const vm = new Vue({
        el: '#app',
        render: (h) => {
            return h(HelloWorld)
        }
    });
等同于：
const vm = new Vue({
        render: (h) => {
            return h(HelloWorld)
        }
    });
vm.$mount('#app');
```
 
 * data-内部属性
 支持对象和函数，优先函数<br>
 函数：
 ```
    data() {
        return {n: 0}
    },
```
对象：
```
data:{
    n:0
}
```
 
 * methods-方法
 事件处理函数或是普通函数
 举例：
 ```
    <div>{{filter(array)}}</div>

    methods: {
        filter() {
            return this.array.filter(i => i % 2 === 0)
        }
    }
```
但是注意：每次渲染都会执行该函数
 
 * components
使用Vue组件，举例：
```
import My from "./components/My"

    components:{
      Add2:My
    },

    template:`        
        <Add2/>
    `,
```
ES6新语法规定，如果components的两边相等的话，可以简写，举例：
```
    components:{MY},
    template:`
        <My/>
        `
```

即可将My组件添加到其他组件中，另一种方法：
```
Vue.component('Demo2',{
   template: `
   <div>demo2</div>
   `
});

同样可以通过：
template:`
  <Demo2/>
`
的方式导入该组件
```
第三种写法，将组件写在组件中：
```
    components: {
        Add3: {
            data() {
                return {n: 1}
            },
            template: `
                <div>{{n}}
                    <button @click="add">+3</button>
                </div>
            `,
            methods: {
                add() {
                    this.n += 3;
                }
            }
        }
    },
```
这样也可以直接在外面使用Add3组件。

注意：组件名的命名方式：

* 1.kebab-case (短横线分隔命名)
* 2.PascalCase (首字母大写命名) 


 
 * 四个钩子<br>
  created-实例出现在内存中<br>
  mounted-实例出现在页面中<br>
  updated-实例更新<br>
  destroyed-实例消亡
  
  举例：
  ```
main.js:

import App from './App.vue'
import My from "./components/My"
import Add3 from "./components/Add3";

const Vue = window.Vue;

Vue.component('Demo2', {
    data(){
      return{
          isShow:true
      }
    },
    components: {
      Add3
    },
    template: `
   <div>
   <div v-if="isShow">
      <Add3/>
   </div>
   <button @click="toggle">显示/隐藏</button>
   </div>
   `,
    methods: {
        toggle(){
            this.isShow = !this.isShow;
        }
    }
});

const vm = new Vue({
    data: {
        array: [1, 3, 5, 4, 5, 6, 7, 8]
    },
    components:{
      Add3
    },
    template: `
    <div>
        <Demo2/>
    </div>
    `,
    methods: {
        filter() {
            return this.array.filter(i => i % 2 === 0)
        }
    }
}).$mount('#app');



// createApp(My).mount('#app');

Add3.vue:

<script>
    export default {
        data() {
            return {
                n: 1
            }
        },
        template:`
            <div>{{n}}
                <button @click="add">+3</button>
            </div>

        `,
        created() {
            console.log("创建对象至内存中")
        },
        mounted() {
            console.log("完成对象的挂载")
        },
        updated() {
            console.log("更新了")
        },
        destroyed() {
            console.log("对象删除")
        },
        methods: {
            add() {
                this.n += 3;
            }
        }

    }
</script>

<style scoped>

</style>

```
点击+3会，控制台会输出更新数据，点击隐藏会输出对象删除，并且注意，新生成的对象不是之前的对象。

* props -外部属性
定义组件：
```
<script>
export default {
  name: 'HelloWorld',
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
组件中的 msg 和message从外部传入，例如：`<HelloWorld msg="msg" message="message"/>`,需要注意，如果传入属性时，使用 `:msg`那么后面可以饥接js代码，例如：
```

 data() {
        return {
            isShow: true,
            msg1:"从data中获取的第一部分",
            msg2:"从data中获取的第二部分",
        }
    },

<HelloWorld :msg="msg1+msg2" message="message"/>
```
当然，里面还可以传入方法：
```
 <HelloWorld :msg=" getStr() " message="message"/>

    methods: {
        getStr(){
            return "从方法中获取"
        }
    }
```