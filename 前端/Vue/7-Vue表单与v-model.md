#### 1. 基本用法
可以直接看文档：<a href="https://cn.vuejs.org/v2/guide/forms.html">表单输入绑定</a>

* input
```
    <div>
        <input v-model="message" placeholder="edit me">
        <p>Message is: {{ message }}</p>
    </div>
```
* textarea
```
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```
* checkbox
```
    <label>抽烟
        <input type="checkbox" v-model="checked" value="smoking">
    </label>
    <label>喝酒
        <input type="checkbox" v-model="checked" value="drinking">
    </label>
    <label>烫头
        <input type="checkbox" v-model="checked" value="haircut">
    </label>
    <p>{{checked}}</p>
```
* radio
```
    <label>抽烟
        <input type="radio" v-model="checked" value="smoking">
    </label>
    <label>喝酒
        <input type="radio" v-model="checked" value="drinking">
    </label>
    <label>烫头
        <input type="radio" v-model="checked" value="haircut">
    </label>
    <p>{{checked}}</p>
```

* select-下拉选项

单选：
```
    <div>
        <select v-model="checked">
            <option disabled value="">请选择</option>
            <option>抽烟</option>
            <option>喝酒</option>
            <option>烫头</option>
        </select>
        <p>{{checked}}</p>
    </div>
```

多选：
```
    <div>
        <select multiple v-model="checked">
            <option disabled value="">请选择</option>
            <option>抽烟</option>
            <option>喝酒</option>
            <option>烫头</option>
        </select>
        <p>{{checked}}</p>
    </div>
```
当我们想把值绑定为其他内容时，可以用v-bind(简写为：),例如：
```
    <div>
        <select multiple v-model="checked">
            <option disabled value="">请选择</option>
            <option :value="1">抽烟</option>
            <option :value="2">喝酒</option>
            <option :value="3">烫头</option>
        </select>
        <p>{{checked}}</p>
    </div>
```
引入v-for:
```
    <div>
        <select multiple v-model="checked">
            <option disabled value="">请选择</option>
            <option v-for="option in options" :key="option.id" :value="option.id">
                {{option.value}}
            </option>
        </select>
        <p>{{checked}}</p>
    </div>
```
data
```
        data() {
            return {
                options: [
                    {id: '1', value: "抽烟"},
                    {id: '2', value: "喝酒"},
                    {id: '3', value: "烫头"}
                ],
                checked:[],
                message: ''
            }
        },
```

* from - 表单

```
    <div>
        //阻止默认动作，防止刷新页面
        <form @submit.prevent="onSubmit">
            <label>
                <span>用户名：</span>
                <input type="text" v-model="user.username">
                <button type="button">确认</button>
            </label>
            <label>
                <span>密码：</span>
                <input type="password" v-model="user.password">
            </label>
            //提交按钮标准写法
            <button type="submit">登录</button>
        </form>
    </div>
```
##### 1.1 修饰符

* .lazy

举例：
```
            <label>
                <span>用户名：{{user.username}}</span>
                <input type="text" v-model.lazy="user.username">
            </label>
```
加上.lazy修饰符后，对文本的input事件，不会改变外面username的值，触发change事件则会修改username的值
* .number
只要数字，对于开头的0以及输入其他不合法数字，都会被parseInt
```
            <label>
                <span>序号：</span>
                <input type="text" v-model.number="user.number">
            </label>
```

* .trim
去除开头结尾的空格，常用于输入username中
```
            <label>
                <span>用户名：{{user.username}}</span>
                <input type="text" v-model.lazy.trim="user.username">
            </label>
```

##### 1.2 v-model
v-model包含两部分，一部分为输入事件的监听，一部分为属性的绑定，例如：
v-model可以写为
```
 <input type="text" :value="user.username" @input="user.username = $event.target.value">
```