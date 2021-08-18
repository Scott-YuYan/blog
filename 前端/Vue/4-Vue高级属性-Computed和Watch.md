#### 1.进阶属性
 
 ##### 1.1 computed-计算属性
 被计算出来的属性就是计算属性，简单示例：
 
 ```
new Vue({
    template: `
        <div>
            <div>{{computedProperty}}</div>
        </div>
    `,
    computed:{
        computedProperty() {
            return "计算属性";
        }
    }
}).$mount("#app");
```

计算属性更加明显的例子：
```
let id = 0;
const createUser = (name, gender) => {
    id += 1;
    return {id: id, name: name, gender: gender}
};

new Vue({
    data: {
        users: [
            createUser("张三", "M"),
            createUser("李四", "F"),
            createUser("王五", "M"),
            createUser("赵六", "F"),
            createUser("孙七", "M")
        ],
        gender: ''

    },
    computed: {
        displayUser() {
            const {users, gender} = this;//析构写法
            if (gender === '') {
                return users;
            } else if (gender === 'M' || gender === 'F') {
                return users.filter(user => user.gender === gender);
            } else {
                throw new Error("参数错误");
            }
        }
    },
    template: `
        <div>
            <div>
                <button @click="setGender('')">全部</button>
                <button @click="setGender('M')">男</button>
                <button @click="setGender('F')">女</button>
            </div>
            <ul>
                <li v-for="user in displayUser" :key="user.id">
                    {{user.id}}-{{user.name}}-{{user.gender}}
                </li>
            </ul>
        </div>
    `,
    methods: {
        setGender(gender) {
            this.gender = gender;
        }
    },

}).$mount("#app");
```
需要知道的是，computed是默认有缓存的，如果以来的属性没有变化，则不会重新计算 

##### 1.2 watch-侦听 - 用于监听数据，当数据变化时执行函数
举例-使用watch属性监听数据变化：
```
<script>
    export default {
        data() {
            return {
                number: 0,
                history: [],
                inUndoModel: false
            }
        },
        methods: {
            add1() {
                this.number += 1;
            },
            minus1() {
                this.number -= 1;
            },
            undo() {
                const lastData = this.history.pop();
                if (lastData !== undefined) {
                    this.inUndoModel = true;
                    this.number = lastData.oldValue;
                    // setTimeout(() => {
                    //     this.inUndoModel = false;
                    // }, 0)
                    this.$nextTick(()=>{
                       this.inUndoModel = false;
                    });
                }
            }
        },
        template: `
            <div>
                <div>{{number}}</div>
                <div>
                    <button @click="add1">+1</button>
                    <button @click="minus1">-1</button>
                    <button @click="undo">撤销</button>
                </div>
                <div>
                    {{history}} 
                </div>
            </div>
        `,
        watch: {
            number(newValue, oldValue) {
                if (!this.inUndoModel) {
                    this.history.push({oldValue: oldValue, newValue: newValue})
                }
            }
        }
    }
</script>

<style>

</style>
```
对象属性从无到有的过程，不会触发watch监听事件，除非在watch中添加`immediate=true`。需要注意的是：不要在watch中使用箭头函数，由于箭头函数中没有this,所以this不是指向的Vue示例对象。
<br>
<a href="https://cn.vuejs.org/v2/api/#watch>Vue-watch</a>中介绍了watch可选的参数类型。另外，watch可以写在new Vue()外部，举例：
```
const vm = new Vue({...});
vm.#watch('n',function(){...},{immediate:true...})
```

##### 1.3 computed与watch的异同

* 翻译：computed-计算属性 watch-监听属性
* 二者都是用于用于监听数据变化，在数据变化时执行函数，computed更注重函数计算的结果，而watch更侧重属性变化时，执行函数。

需要注意的是：
* computed调用时不需要加括号，并且存在缓存，如果属性没有变化，则不会重新计算。
* watch有两个属性 immediate: 设置是否首次加载时执行函数; deep: 是否监听对象内部值的变化。
 
 #### 2.何谓数据变化
 举例：
 ```
<script>
    export default {
        data() {
            return {
                n: 0,
                p: {
                    name: '',
                    age: ''
                }
            }
        },
        methods: {},
        template: `
            <div>
                <div>
                    {{n}}
                    <hr>
                    {{p}}
                </div>
                <button @click=" n = n+1">+1</button>
                <button @click=" p.name+='hi'">变名称</button>
                <button @click=" p = {name:'zhangsan',age:'18'}">变对象属性</button>
                <button @click=" p = {name:'',age:''}">变对象地址</button>
            </div>
        `,
        watch: {
            n() {
                console.log('n变化了');
            },
            p() {
                console.log('p变化了');
            },
            "p.name"() {
                console.log('p.name 变化了');
            }
        }
    }
</script>
```
等同于`===`的规则，如果想侦听对象中的任一属性变化，可以使用`deep=true`属性，例如：
```
            p: {
                handler() {
                    console.log('p变化了');
                },
                deep:true
            },
```