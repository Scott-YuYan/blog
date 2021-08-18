#### 1. 函数与延迟执行
看下面的代码：
```
class Component_New extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            id: 0
        }
    }

    addId = () => {
        this.setState({id: this.state.id + 1});
        this.setState({id: this.state.id + 1});
    };

    render() {
        return <div>
            id:{this.state.id}
            <button onClick={this.addId}>Add</button>
        </div>
    }
}

```
点击组件中的Add按钮，id是多少？
答案：还是1
<br>
由于addId函数是延迟执行的，所以在setState中修改id的值，id不会立即做出改变，需写成：
```
    addId = () => {
        this.setState((state) => {
            return {id: state.id + 1}
        });
        this.setState((state) => {
            return {id: state.id + 1}
        });
    };
```
改为函数调用