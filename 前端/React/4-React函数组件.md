#### 1.创建方式

* 方式一：
```
const Hello = (props) => {
    return <div>{props.message}</div>
}
```
* 方式二：
```
const Hello = props => <div>{props.message}</div>
```

* 方式三：
```
function Hello(props){
    return <div>{props.message}</div>
}
```
函数组件最大的优点就是简洁，例如实现一个简单的+1功能，<br>
对于类组件：
```
class App extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            n: 0
        };
    }

    addN = () => {
        this.setState((state) => {
            return {n: state.n + 1}
        })
    };

    render() {
        return <div>
            {this.state.n}
            <button onClick={() => this.addN()}>n+1</button>
        </div>
    }
}
```
<br>
对于函数组件:

```
import React, {useState} from 'react';

const App = (props) => {
    const [n, setN] = useState(0);
    const addN = () => {
        setN(n + 1);
    };
    return <div>
        {n}
        <button onClick={addN}>+1</button>
    </div>
};
```
显而易见的区别。(n跟setN都是随便写的，完全可以叫其他名字)<br>

#### 2.函数组件中的生命周期
React中，使用useEffect解决生命周期的问题，举例：

* 模拟componentDidMount
```
import React, {useState, useEffect} from 'react';

    useEffect(() => {
        console.log("use Effect")
    }, []);
// []表示第一次渲染的时候执行
```

* 模拟componentDidUpdate
```
    useEffect(()=>{
        console.log("任意属性更新")
    });

    useEffect(()=>{
        console.log("state更新了");
    },[state]);
```

* 模拟componentWillUnmount

```
    useEffect(()=>{
        return ()=>{
            console.log("组件要消失了")
        }
    });

    const show = () => {
        setVisible(true);
    };

    const hidden = () => {
        setVisible(false);
    };

    return <div className="son">
        {visible ? <div>Son</div> : null}
        {visible ? <button onClick={hidden}>hidden</button> : <button onClick={show}>show</button>}
    </div>
```

##### 2.1 其他生命周期如何模拟

* constructor
 函数组件执行的时候，就相当于constructor
 
* shouldComponentUpdate
 后面的React.memo 和useMemo 可以解决，例如：
 ```
const MyComponent = React.memo(function MyComponent(props) {
  /* 使用 props 渲染 */
});
```
 
* render
函数组件的返回值就是render的返回值

#### 3.React Hooks

##### 3.1 useRef
看下面的例子：
```
import React, {useState, useEffect, useRef} from 'react'

const UseRef = (props) => {
    const [n, setN] = React.useState(0);

    const add = () => {
        setN(n + 1);
    };

    const log = () => {
        setTimeout(() => {
            console.log("n的值为:" + n);
        }, 3000)
    };

    return (<div>
        <span>n:{n}</span>
        <div>
            <button onClick={add}>+1</button>
        </div>
        <div>
            <button onClick={log}>log</button>
        </div>
    </div>);
};

export default UseRef;
```
先点击add再点击log，可以正常打印日志，但是，先点击log，然后再三秒内点击add，log中打印的还是0。究其原因，由于React的设计思想
就是避免直接修改传入的参数。所以点击add按钮后，n(值为0),n(值为1)是两个n。
<br>
如何解决呢？

* 1.可使用react.useRef,举例：
```
import React, {useState, useEffect, useRef} from 'react'

const UseRef = (props) => {
    const [n, setN] = React.useState(0);
    const nRef = React.useRef(0);
    const update = React.useState(null)[1];

    const add = () => {
        nRef.current += 1;
        update(nRef.current);
    };

    const log = () => {
        setTimeout(() => {
            console.log(`n的值为 n:${nRef.current}`);
        }, 3000)
    };

    return (<div>
        <span>n:{nRef.current}</span>
        <div>
            <button onClick={add}>+1</button>
        </div>
        <div>
            <button onClick={log}>log</button>
        </div>
    </div>);
};

export default UseRef;

```

 
* 2.可以使用React.useContext
举例：
```
import React from "react";
import ReactDOM from "react-dom";
import "./style.css";
const themeContext = React.createContext(null);

function UseContext() {
    const [theme, setTheme] = React.useState("red");
    return (
        <themeContext.Provider value={{ theme, setTheme }}>
            <div className={`App ${theme}`}>
                <p>{theme}</p>
                <div>
                    <ChildA />
                </div>
                <div>
                    <ChildB />
                </div>
            </div>
        </themeContext.Provider>
    );
}

function ChildA() {
    const { setTheme } = React.useContext(themeContext);
    return (
        <div>
            <button onClick={() => setTheme("red")}>red</button>
        </div>
    );
}

function ChildB() {
    const { setTheme } = React.useContext(themeContext);
    return (
        <div>
            <button onClick={() => setTheme("blue")}>blue</button>
        </div>
    );
}

export default UseContext;
```

React.createContext可以贯穿整个App中引用的组件。