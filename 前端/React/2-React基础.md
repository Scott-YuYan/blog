* 1.类组件和函数组件
 函数组件：<br>
 ```
 function Welcome(props){
    return <h1> Hello , {props.name}</h1>;
 }
```
注意： `return <h1>...` 会被翻译为React.createElement(...)。
使用方法：`<Welcome name='zhansan'/>`

类组件：<br>
```
class Welcome extends React.Component {
    constructor(){
        super()
        this.state = {n:0}
    }
    render(){
        return <h1>Hello,{this.props.name}</h1>
    }
}
```
使用方法同上

* 2.props和state

##### 2.1 props-用于接收外部数据 & 用于接收外部函数
props与Vue中的props类似，用于接收一个外部数据，使用方法如下：<br>
对于函数组件
```
传入：<Son messageForSon = "我来自外层"/>
接收：{props.messageForSon}
```
对于类组件：
```
传入：<GrandSon messageForGrandSon="我来自儿子组件"/>
接收：{this.props.messageForGrandSon}
```

类组件举例：
```
class Outer extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            name: 'Outer',
            id: 1
        }
    }

    addId = () => {
        this.setState({id: this.state.id += 1})
    };

    render() {
        return <div>Outer
            <Inner name={this.state.name} id={this.state.id} addId={this.addId}/>
        </div>
    }
}

class Inner extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            name: 'Inner'
        }
    }

    render() {
        return <div>{this.props.name}:{this.props.id}
            <button onClick={
                () => {
                    this.props.addId();
                }
            }>addId
            </button>
        </div>
    }
}

//外部数据会被包装称为一个对象：{name:"Outer",showName = {this.showName}}
// 此处的addId是一个回调
// 注意，永远不要修改propos的值，外部数据就应该由外部更新
```




##### 2.2 state-内部数据
对于函数组件：
```
 const [n, setN] = React.useState(0); // 声明n与setN()方法
```
使用：
```
    return <div className="son">
        n:{n}
        <button onClick={() => {
            setN(n + 1)
        }}>+1
        </button>
    </div>
```

对于类组件：
```
class GrandSon extends React.Component {
    constructor() {
        super();
        //声明state
        this.state = {
            n: 0
        }
    }

    render() {
        return <div className='grandSon'> n：{this.state.n}
            <button onClick={() => {
                this.add()
            }}>+1</button>
        </div>
    }

    add() {
        this.setState({n: this.state.n + 1});
    }
}

```
但是需要注意，如果在add中加上日志，打印n的值：
```
    add() {
        this.setState({n: this.state.n + 1});
        console.log(this.state.n)
    }
```
就可以发现，点击按钮后，居然打印的还是0。原因在于`setState()是异步的，他会等console.log执行完毕后再更新值`。
所以，更推荐在setState()中使用函数：
```
    add = ()=> {
        this.setState((state) => {
            const newState = state.n + 1;
            console.log(newState);
            return {n: newState}
        });
    }
```

##### 2.3 setState使用注意事项
 1. 使用`this.state.n +=1`页面数据没有变化？ 原因在于：this.state.n+=1;可以改变数据，但是不会更新UI，所以需要使用setState()
<br>
 2. setState()是异步的,设置值之后，state不会立马改变。
 <br>
 3. 不推荐使用this.setState(this.state),React不希望我们修改旧数据，而是用新数据去覆盖
 <br>
 4. setState((state,props) => {newState,fn}),fn会在setState成功后执行，举例：
 ```
        this.setState((state) => {
            return {id: state.id + 1}
        },()=>{console.log("修改成功")});
```
 5. 如果state中有多个对象，更新其中一个对象的时候，需不需要传入其他对象的值？

 
 举例：
 ```
// 类组件
class GrandSon extends React.Component {
    constructor() {
        super();
        this.state = {
            n: 0,
            m: 0
        }
    }

    render() {
        return <div className='grandSon'>孙子 n：{this.state.n}，m:{this.state.m},从外部接收数据：{this.props.messageForGrandSon}
            <button onClick={() => {
                this.addN()
            }}>n+1
            </button>
            <button onClick={() => {
                this.addM()
            }}>m+1
            </button>
        </div>
    }

    addN() {
        this.setState((state) => {
            const newState = state.n + 1;
            return {n: newState}
        });
    }

    addM() {
        this.setState((state) => {
            const newState = state.m + 1;
            return {m: newState}
        });
    }
}
```
可以看到，对state中的某个值修改时，不用关心其他值，相当于：`this.setState({...this.state, n: this.state.n + 1});`
<br>
那么对于函数组件呢,当然可以使用：
```
    const [n, setN] = React.useState(0);
    const [m, setM] = React.useState(0);
```
使用对象呢：
```
//函数组件
function Son(props) {
    const [state, setState] = React.useState({m: 0, n: 0});
    return <div className="son">
        n:{state.n} , m:{state.m}
        <button onClick={() => {
            setState({n: state.n + 1})
        }}>n+1
        </button>
        <button onClick={() => {
            setState({m: state.m + 1})
        }}>m+1
        </button>
    </div>
}
```
这种写法就可以发现，修改m的时候，n的值会被undefine覆盖，反之依然。所以可以得出结论，函数组件修改某个值时不会自动保留其他属性，
但是类组件可以(注意:也只是保留第一层属性，如果某个属性有多层，那么内容还是会被覆盖)，但是还是更推荐使用函数组件，更简洁。上面的写法改为：
```
        <button onClick={() => {
            setState({...state, n: state.n + 1})
        }}>n+1
        </button>
        <button onClick={() => {
            setState({...state, m: state.m + 1})
        }}>m+1
        </button>

```
使用...state保留原来的值即可。
#### 3.绑定事件

##### 3.1 类组件的事件绑定：
1.写法一(常用)：
```
<button onClick = {()=>{this.addN()}}>addN</button>
```

2.写法二：
```
<button onClick = {this.addN()}>addN</button>
```
这种写法的问题在于，会将this.addN里面的this变为window。原因在于addN被点击时，React实际调用的是：
```
button.onClick.call(null,event)
```
而null会被补位成window

3.写法三：
```
<button onClick = {this.addN.bind(this)}>addN</button>
```
返回一个绑定了当前this的新函数，这种写法太麻烦

4.写法四-最终写法：
```
将函数写在constructor外面：
addN = ()=>{ this.setState({n:this.state.n}) }
```
这样可以直接调用：
```
<button onClick = {()=> this.addN()}
```