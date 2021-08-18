#### 0.前言
该部分深入介绍vue属性中的data部分，<a href="https://cn.vuejs.org/v2/guide/reactivity.html">Vue官方文档中</a>，有该部分的介绍。


#### 1.getter和setter
vue的文档中提到，"当你把一个普通的 JavaScript 对象传入 Vue 实例作为 data 选项，Vue 将遍历此对象所有的 property，并使用 Object.defineProperty 把这些 property 全部转为 getter/setter。"
何为getter/setter，举例：

```
const myDate = {
    n: 0
};

console.log(myDate);

new Vue({
    data: myDate,
    template: `
        <div>{{n}}</div>
    `
}).$mount("#app");

setTimeout(()=>{
    console.log(myDate)
},3000);
```

打开控制台可以看到：

![image](https://user-images.githubusercontent.com/51253421/124935758-8939f780-e038-11eb-81b1-085012a0f292.png)

可以看到myDate对象由`{n:0}`变为了`{n:...},这就需要知道ES6新增的<a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Object_initializer#ECMAScript_6%E6%96%B0%E6%A0%87%E8%AE%B0">getter和setter</a>,
首先，有这样一个对象：
```
let obj0 = {
  姓: "高",
  名: "圆圆",
  age: 18
};
```

需求一，得到姓名，代码如下：
```
let obj0 = {
  姓: "高",
  名: "圆圆",
  age: 18,
  姓名(){
  return this.姓+this.名;
}
};
```
通过调用姓名()函数可以获取该对象，那么可不可以不加这个括号呢？引入ES6的新语法getter函数：
```
let obj2 = {
  姓: "高",
  名: "圆圆",
  get 姓名() {
    return this.姓 + this.名;
  },
  age: 18
};
```
可以直接用obj2.姓名获取，同时加入setter:
```
let obj3 = {
  姓: "高",
  名: "圆圆",
  get 姓名() {
    return this.姓 + this.名;
  },
  set 姓名(xxx){
    this.姓 = xxx[0]
    this.名 = xxx.slice(1)
  },
  age: 18
};
```
可以通过 obj3.姓名 = 'xxx'设置属性，这时候打出obj3对象：

![image](https://user-images.githubusercontent.com/51253421/124941752-9f968200-e03d-11eb-9cd5-0263db071d03.png)

可以看到obj3.姓名属性也为`(...)`,说这个姓名并不是一个真实的属性，而是通过`get/set 姓名`来模拟对姓名的操作。

#### 2.Object.defineProperty
 "Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。
 "<a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty"> - MDN文档</a>
 文档中给出的示例： 
 
 1. 为对象新增属性并赋值
 ```
const object1 = {};

Object.defineProperty(object1, 'property1', {
  value: 42
});
```

 
 2.对象新增一个getter、setter方法
 ```
// 在对象中添加一个设置了存取描述符属性的示例
var bValue = 38;
   Object.defineProperty(o, "b", {
     // 使用了方法名称缩写（ES2015 特性）
     // 下面两个缩写等价于：
     // get : function() { return bValue; },
     // set : function(newValue) { bValue = newValue; },
     get() { return bValue; },
     set(newValue) { bValue = newValue; },
     enumerable : true,
     configurable : true
   });
```
b属性不存在于对象o中，所以无法使用o.b获取该属性。需要注意的是，如果对象中已经有了某个属性，
再用Object.definedProperty(对象,"属性",{...})时，原属性会被覆盖。

#### 3.代理和监听
上一个例子中,我们给对象o新增了一个虚拟对象b,但是问题在于，我们可以通过修改bValue的值，修改o.b的值。那么如何
避免这种情况，这就需要使用代理：
```
function proxy({data}) {
    const obj = {};
    Object.defineProperty(obj, 'n', {
        get() {
            return data.n;
        },
        set(value) {
            data.n = value;
        },
    });
    return obj;
};
```
赋值：
```
let newData = proxy({data:{n:10}});
newData.n获取n的值，并且无法通过其他方式修改n的值
```
前面我们使用的是匿名对象，那么如果用具名对象：
```
let nameData = {n:10};
let newData =  proxy({data:nameData});
```
那么还是可以通过修改nameDate的值修改obj的属性。这时候就可以通过Object.defineProperty对nameData增加监控,
再结合proxy()函数，即可从数据源增加赋值条件。
```
function proxy({data}) {
     let value = data.n; //先取出data.n的值，避免后面被覆盖后丢失
     Object.defineProperty(data, "n", {
         get() {
             return value;
         },
         set(aValue) {
             if (aValue > 0) {
                 this.n = aValue;
             }
         }
     });
     
     const obj = {};
     Object.defineProperty(obj, 'n', {
         get() {
             return data.n;
         },
         set(value) {
             data.n = value;
         },
     });
     return obj;
 }
```

#### 4.与Vue的关系
`let newData = proxy({data:{n:10}});` 这其实就类似与我们创建Vue示例中用的：

```
let myData = {n:0};
let app = new Vue({
  data:myData
});
```
那么同样的，Vue让app成为了myData的代理，并且对myData的属性进行监控。当myData的属性发生修改时，调用render(data)方法刷新页面。

#### 5.数据响应式
`const vm = new Vue({data:{n:0}})`,vm.n修改那么UI中的n也会做出修改。Vue2通过Object.defineProperty实现数据响应式。如果我改变窗口的大小，网页内容会做出响应，那么就是响应式网页。比如：
https://www.smashingmagazine.com/

##### 5.1Vue的一个bug
由于Object.defineProperty(obj,'n',{...}),必须要求有一个n,才能监听&代理obj.n,那如果像下面的情况：
```
new Vue({
    data: {
        obj: {
            n: 0
        }
    },
    template: `
        <div>
            <div>{{obj.a}}</div>
            <button @click="setA">setA</button>
        </div>
    `,
    methods: {
        setA() {
            this.object.a =1;
        }
    }
}).$mount("#app");
```
可以看到,obj中没有a这个属性, 那么Vue也无法监听一开始就不存在的obj.a。解决办法：
* 1.事先将key都声明好,比如实现在obj中声明 a:undefined
* 2.使用Vue.set或者this.$set,那么Vue就会使用Object.definedProperty()对属性增加监听。代码示例：
```
new Vue({
    data:{
        obj:{
            n: 0
        }
    },
    template: `
        <div>
            <div>{{obj.a}}</div>
            <button @click="setA">setA</button>
        </div>
    `,
    methods:{
        setA(){
            // this.object.a =1;
            this.$set(this.obj,'a',1)
        },

    }
}).$mount("#app");
```
setA之后，就可以正常的修改obj.a属性了
Vue.set/this.$set做了以下几件事情：
* 新增key
* 自动创建代理和监听(如果没有创建过)
* 触发UI更新(但是不会立即更新)

##### 5.2 数组的变异方法
接着前面的例子，我们可以提前声明obj.a对象为undefined来处理,但是问题来了，不是所有的对象都可以提前声明，比如数组，看下面的例子：
```
new Vue({
    data:{
        array:['a','b','c']
    },
    template: `
        <div>
            <div>{{array}}</div>
            <button @click="setA">setA</button>
        </div>
    `,
    methods:{
        setA(){
            // this.object.a =1;
            this.array[3] = 'd';
        }
    }
}).$mount("#app");
```
由于Vue不知道数组的第四个对象，所以这时候点击按钮是没有效果的，可以将第四位设置为undefined，但是数组个数不确定，所以解决方案：
* 1.按照前面的例子，使用`Vue.set/this.$set`:`this.$set(this.array,3,'b')`
* 2.使用Vue修改后的push方法， `this.array.push('b');`,将array打印出来后，可以看到array的原型与数组的原型有所区别：

![image](https://user-images.githubusercontent.com/51253421/125149839-fbf8bf00-e16d-11eb-8947-4288909b9428.png)

Vue会在数组的原型之外，再加上一层原型，<a href="https://cn.vuejs.org/v2/guide/list.html#%E5%8F%98%E6%9B%B4%E6%96%B9%E6%B3%95">Vue文档-变更方法中也提到了该部分</a>


