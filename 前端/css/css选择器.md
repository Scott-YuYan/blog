<h3>基本选择器</h3>

* 1.通用选择器 `*` 

* 2.元素选择器 例：`div`或`input`

* 3.类选择器 [`.classname`] 例：`.eye`

* 4.ID选择器 [`#idname`] 例:`#eye`

* 5.属性选择器[`[attr] [attr=value] [attr~=value] [attr|=value] [attr^=value] [attr$=value] [attr*=value]`]

例：[autoplay] 选择所有具有 autoplay 属性的元素

<h3>分组选择器</h3>

* 分组选择器 `,` 例：`A,B`同时选择`A和B`

<h3>组合器</h3>

* 1.后代选择器  ` ` 例：`A B` 选择A后代中所有的B

* 2.直接子代选择器 `>` 例：`A>B` 选择A直接子代中的B
举例：
```
    <div class="class1">
        <div class="class2">
            <div class="class3"></div>
        </div>
    </div>
```
使用`.class1 .class3`可以选中元素，使用`.class1>.class3`则没有元素选中

* 3.一般兄弟组合器 `~` 后一个节点在前一个节点的后面任意位置，且共享同一个父节点

* 4.紧邻兄弟组合器 `+` 后一个元素紧跟在前一个之后，并且共享同一个父节点

举例：
```
            <div class="class3"></div>
            <div class="class4"></div>
            <div class="class5"></div>
```
使用`.class3~.class5`可以选中元素，使用`.class3+.class5`则无法选中

<h3>伪选择器</h3>
* 伪类`:` 伪选择器支持按照未被包含在文档树中的状态信息来选择元素。
![image](https://user-images.githubusercontent.com/51253421/118909919-4b80f280-b956-11eb-8816-d704189245ae.png)

* 伪元素`::` 伪选择器用于表示无法用 HTML 语义表达的实体。
![image](https://user-images.githubusercontent.com/51253421/118909994-69e6ee00-b956-11eb-9299-152d895a8f1a.png)

<h3>Tips:</3>
* 1.`.A.B(中间无空格)`与`.A .B(中间有空格)`的区别？
前者是选中class中即有A又有B的class元素，例如`class="A B"`,后者是选中`class="A"`下面的所有`class="B"`