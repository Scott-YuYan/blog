<h3>MVC是什么</h3>
首先，MVC是一种设计模式，所有的页面都可以用MVC来优化代码结构。具体来说，就是每一个模块都可以写成三个对象，分别是M、V、C:

* M-Model(数据模型)负责操作所有数据
* V-View(视图)负责所有UI界面
* C-Controller(控制器)负责其他

在模块拆分的过程中，我们可以逐渐意识到一些优化代码的原则：

* 1. 最小知识原则
即在写这个模块的时候，我们应尽量减少对其他模块中数据的引用。<br>举例：
原本引入一个模块，需要引入`HTML+CSS+JS`，但当我们在js中引入css后，并将html代码，在js中用字符串保存之后，那么一个模块就对应
一个js。引入这么模块，我们只需要知道他的js代码即可。

* 2. 以不变应万变
既然任何一个模块，我都可以用MVC分对象实现，那么对于其他需要，我们可以照搬结构。Vue将这三部分进行了浓缩，但是我们还可以看到
Vue中的el，就对应v.el,data对应m.data。

* 3. 表驱动编程
对于代码中的大批量的重复，一般其中相差的就是一两个字段，那么将这些字段抽取出来做成哈希表。举例：
四个按钮：
```
        <button id="add1">+1</button><button id="minus1">-1</button>
        <button id="multiply2">*2</button><button id="divide2">/2</button>
```
需要对这四个按钮增加事件监听，原本需要
```
//用jQury
$(#add1).on('click',()=>{...})
$(#minus1).on('click',()=>{...})
$(#multiply2).on('click',()=>{...})
$(#divide2).on('click',()=>{...})
```
将css选择器与调用的函数赋名，将这些字段抽取出来，放到hash表中：
```
    events: {
        'click #add1': 'add',
        'click #minus1': 'minus',
        'click #multiply2': 'multiply',
        'click #divide2': 'divide'
    },
```
并通过对events进行遍历：
```
        for (let key in c.events) {
            let value = c[(c.events[key])];
            let array = key.split(" ");
            v.el.on(array[0], array[1], value);
        }
```
完成事件监听的绑定。这种思想，在Vue中的watch中也有体现。

* 4.事不过三
同样的代码写三遍，就应该抽取成一个函数;同样的属性写三遍，就应该做成公共属性(原型或类);同样的原型写三遍就应该用继承;

* 5.俯瞰全局
把所有的对象看成点，点与点之间的通信，用一个专门的对象来负责，这个对象就称为eventBus。

* 6.view = render(data)
对页面中的对象进行修改，由之前的先找到dom元素，然后修改dom元素内容过渡到，页面上用占位符与数据进行绑定，然后通过修改数据并刷新
页面的方式，实现修改。React就是这么做的，Vue对这部分进行了优化，即完成了数据绑定后，修改页面只会刷新修改部分的dom，而不是整个页面。