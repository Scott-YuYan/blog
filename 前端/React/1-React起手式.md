#### 1.如何引入React

##### 1.1 CDN引入(了解)
* 1.引入React:https://.../react/x.min.js
* 2.引入ReactDOM:https://.../react-dom.x.min.js

```
    <script src="https://cdn.bootcss.com/react/16.10.2/umd/react.development.js"></script>
    <script src="https://cdn.bootcss.com/react-dom/16.10.2/umd/react-dom.development.js"></script>
```
CDN引入的React会成为全局变量。

* 3.注意cjs和umd的区别：
<ul>
<li>1.cjs全称CommonJS,是Node.js支持的模块规范</li>
<li>2.umd是同一模板定义，兼容各种模块规范(含浏览器)</li>
<li>理论上优先使用umd，同时支持Node.js和浏览器</li>
</ul>

##### 1.2通过webpack引入React
```
yarn add react react-dom
import React from 'react'
import ReactDOM from 'react-dom'
```
注意大小写，上面这种方式可以使用create-react-app代替

##### 1.3 create-react-app
安装命令：`yarn global add create-react-app` 然后使用`create-react-app 路径`在指定位置创建项目

#### 2. 函数与延迟执行

我们知道，声明`let a = b+1`,b+1会立即执行，如果我们声明函数let f = ()=>{b+1}; a = f();那么只有在函数被调用时，才会执行。
同样的道理放在React中：
```
let n = 0;
const Root = document.querySelector('#root');
const App = React.createElement('div', {className: 'red'}, [n,
    React.createElement('button', {
        onClick: () => {
            n += 1;
            ReactDOM.render(App, Root);
        }
    }, "+1")
]);
ReactDOM.render(App, Root);
```

App是一个React元素，点击+1按钮，n的值是不会变化的。但是，如果改为函数调用：
```
const App = () => React.createElement('div', {className: 'red'}, [n,
    React.createElement('button', {
        onClick: () => {
            n += 1;
            ReactDOM.render(App(), Root);
        }
    }, "+1")
]);
ReactDOM.render(App(), Root);
```
App就是React的一个函数组件，n的值是会变化的。<br>

注意:APP并不是真正的div，而是虚拟DOM对象。

#### 3.JSX(JS拓展)

