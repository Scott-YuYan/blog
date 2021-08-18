#### 1.两种创建组件的方式
 * 1.ES5方式(过时)
 
 ```
import React from 'react'

const A = React.createClass({
    render(){
        return <div>Hi</div>
    }
})

export default A
// 由于ES5不支持class，所以才有这种写法
```

* 2.ES6方式-看上一篇文章

#### 2.声明周期

 ##### 2.1 挂载卸载
 
 <ul>
    <li>constructor()</li>    
    <li>componentWillMount()</li>
    <li>componentDidMount()</li>
    <li>componentWillUnmount()</li>
 </ul>
 
 ###### 2.1.1 constructor
 * 初始化props
 * 初始化state，但是此时不能用setState
 * bind this 

 ```
constructor(){
    this.onClick = this.Onclick.bind(this)
}
//用新语法代替
onClick = ()=>{}
constructor() {...}
```

###### 2.1.2 shouldComponentUpdate

用途：
* 返回true表示不阻止UI更新
* false表示阻止UI更新

实际中用于，根据场景灵活的设置返回值，以避免不必要的更新。举例：
```

    add = () => {
        this.setState((state) => {
            return {id: state.id + 1}
        });
        this.setState((state) => {
            return {id: state.id - 1}
        });
    };

    shouldComponentUpdate(nextProps, nextState) {
        return nextState.id !== this.state.id;
    }
```
对于这种情况，id数值并未做实质性改变的情况，则可以设置不用刷新页面。在这个基础上进行衍生，我们对组件进行遍历，
如果它所有的属性都没有变化，那么就不进行render,这部分React已经内置了，使用PureComponent:
```
class Outer extends React.PureComponent{
    ...
}
```

###### 2.1.3 render
用途：展示视图 return (<div>...</div>)<br>
注意：只能有一个跟元素，如果有两个，则需要用`<React.Fragment>`包起来，可以简写为`<></>`

* render中可以写if else 举例：
```
render() {
        let message;
        if (this.state.id % 2===0){
            message =  <div>偶数</div>
        }else {
            message =  <div>奇数</div>
        }
        return (
            <React.Fragment>
                <div>
                    {message}
                </div>
                <div>
                   
                </div>
            </React.Fragment>
        )
    }
```

* render中可以写 ? :表达式
上面的代码可以简写为：
```
                <div>
                    {this.state.id % 2===0 ?<div>偶数</div>:<div>奇数</div>}
                </div>
```

* render中不可以直接写for循环，需要用数组
```
constrcutor(propos){
    super(propos);
    this.state={
        array:[1,2,3]
    }
}

render(){
    let message = this.state.array.map(
        (n) =>
            <div key={n}>{n}</div>
    );
}
//注意：循环中必须加key
```

###### 2.1.4 componentDidMount()

* 用途
<ul>
<li>组件挂载后（插入 DOM 树中）立即调用</li>
<li>发起加载数据的请求(官方推荐)</li>
</ul>

并且注意：首次渲染会执行该钩子。
<br>
示例，元素加载后，获取该元素的高度：
```
    constructor(props) {
        super(props);
        this.state = {
            width: undefined
        };
        this.divRef = React.createRef();
    }

    render() {
        return <div ref={this.divRef}>
            id:{this.state.id},整个div的高度为{this.state.width}
            <button onClick={this.addId}>Add</button>
        </div>
    }

    componentDidMount() {
        const componentNew = document.querySelector("#component_new");
        const width = componentNew.getBoundingClientRect().width;
        this.setState({width});
    }
```

###### 2.1.5 componentDidUpdate()
* 用途
<ul>
<li>在视图更新后执行代码</li>
<li>在此处也可发起AJAX请求，用于更新数据</li>
<li>首次渲染不会执行此钩子，且必须写在if里,否则可能会导致死循环</li>
</ul>

###### 2.1.6 componentWillUnmount
* 用途
<ul>
<li>组件将要被移出页面然后被销毁时执行代码</li>
<li>unmount过的组件不会再次mount</li>
</ul>

* 注意，如果在componentDidMount里面监听了window scroll、创建了Timer、创建了AJAX请求......那么就需要在componentWillUnmount里面取消

 

##### 2.2 更新
<ul>
<li>componentWillReceiveProps(nextProps)</li>
<li>shouldComponentUpdate(nextProps,nextState)</li>
<li>componentWillUpdate(nextProps,nextState)</li>
<li>componentDidUpdate(preProps,prevState)</li>
<li>render()</li>
</ul>

##### 2.3 新增
<ul>
<li>getDerivedStateFromProps(nextProps, prevState)</li>
<li>getSnapshotBeforeUpdate(prevProps, prevState)</li>
</ul>
