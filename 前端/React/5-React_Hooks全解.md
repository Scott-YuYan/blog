#### 1. useState
* 使用状态
```
const[n,setN] = React.useState(n);
const[user,setUser] = React.useState({name:"F"});
```
<br>
注意事项：
* 1.不可局部更新

```
import React, {useState, useRef} from 'react';

function UseState() {
    const [user, setUser] = useState({id: "1", name: "zhangsan", gender: "M"});
    const conClick = () => {
        setUser({name: "张三"})
    };

    return (
        <div>
            <span>{user.id}</span>
            <span>{user.name}</span>
            <span>{user.gender}</span>
            <button onClick={conClick}>+1</button>
        </div>
    );
}

export default UseState;
```
点击按钮会将其他两个属性都覆盖，需要将onClick改为：
```
    const conClick = () => {
        setUser({...user, name: "张三"})
    };
```

* 新的对象与原对象地址不相同

#### 2.useReducer
用于践行Flux/Redux的思想：
 
 * 1.创建初始值initState
 
 * 2.创建所有操作reducer(state,action)
 
 * 3.传给useReducer,得到读和写的API
 
 * 4.调用写{type:'操作类型'}
 
 举例：
 ```
import React, {useState, useRef, useReducer} from 'react';

const initial = {
    n: 0
};

const reducer = (state, action) => {
    if (action.type === 'add') {
        return {n: state.n + action.number}
    } else if (action.type === 'multiply') {
        return {n: state.n * 2}
    } else {
        throw new Error("unknown type")
    }
};

function UseState() {
    const [state, dispatch] = useReducer(reducer, initial);
    const {n} = state;
    const conClick = () => {
        dispatch({type: 'add', number: 2});
    };
    return (
        <div>
            <span>{n}</span>
            <button onClick={conClick}>+1</button>
        </div>
    );
}

export default UseState;
```
总的来说，useReducer就是useState的复杂版。


#### 3.useContext 
全局变量是全局的上下文，使用例子在上一篇文章中。步骤：

* 1.使用`const themeContext = React.createContext(null);` 创建上下文
* 2.使用`<themeContext.Provider></themeContext.Provider>`圈定作用域
* 3.在作用域内使用`React.useContext(themeContext);`使用上下文

#### 4.useEff
监控的对象变化时执行，上一篇文章中有介绍。需要注意：<br>
如果同时存在多个useEffect，那么会按照出现次序执行。

#### 5.useLayoutEffect-布局副作用
* useEffect在浏览器渲染完成后执行
* useLayoutEffect在浏览器渲染前执行
举例：
```
import React, {useState, useEffect, useLayoutEffect} from 'react'

function UseEff(props) {
    useEffect(() => {
        console.log("effect")
    });

    useLayoutEffect(() => {
        console.log("useLayoutEff")
    });

    return (<div>
    </div>);
}

export default UseEff;
```
可以看到useLayoutEffect先打印出来。

#### 6.memo useMemo

```
import React, {useEffect, useState} from 'react'

function Memo(props) {
    const [n, setN] = React.useState(0);
    const [m, setM] = React.useState(0);

    const addN = () => {
        setN(n + 1)
    };

    const addM = () => {
        setM(m + 1)
    };

    return <div>
        <span>{n}</span>
        <button onClick={addN}>setN</button>
        <span>{m}</span>
        <button onClick={addM}>setM</button>
        <Child data={m}/>
    </div>
}

function Child(props) {
    useEffect(() => {
        console.log("Child更新了")
    });

    return <div>
        Child
    </div>
}

export default Memo;
```
点击setN按钮可以看到Child组件也被刷新了，可通过
```
const MemoChild = React.memo((props) => {
    useEffect(() => {
        console.log("Child更新了")
    });

    return <div>
        Child
    </div>

});
```
将Child包裹在memo中，避免这种情况。但是有个问题，如果传入的对象还包括函数:
```
<MemoChild data={m} onClick={()=>{console.log("Child")}}/>
```
这时候，计算Memo被包裹在React.memo中，由于函数是引用，所以同样会刷新Child。
这就需要用到useMemo:

```
    const childClick = useMemo(() => {
        return () => {  
            console.log("Child")
        }
    }, [m]);
// return返回的对象为缓存的函数，[m]表示m变化时，缓存更新
```

#### 7.useRef - 上一篇文章

#### 8.forwardRef

举例：

上一级组件想将ref传给下一层
```
import React, {useRef, forwardRef} from 'react';

function ForwardRef() {
    const buttonRef = useRef(null);
    return <div>
        <Button1 ref={buttonRef}>按钮</Button1>
    </div>
}

const Button1 = React.forwardRef((props, ref) => {
    return <button ref={ref} {...props}/>;
});

export default ForwardRef;
```
这种写法仅用于函数组件，class组件是可以直接使用的。
