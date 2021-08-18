<h5> 1.e.target与e.currentTarget的区别？</h5>

 * e.target 指向触发事件监听的对象。
 * e.currentTarget 指向添加监听事件的对象。
 
 <h5> 2.方法和函数的区别？</h5>
 
 方法是面向对象的概念，函数是数学概念，二者本质上都是function。在面向对象中，函数必须依附于一个对象，例如obj.fn()，但是由于js糅合
 了面向对象和函数式，所以fn()方法可以单独存在，这时我们可以称fn()为函数。
 
 
 <h5> 3.ES6新语法</h5>
 <a href="https://fangyinghang.com/es-6-tutorials/">ES6新特性列表</a>
 
 <h5> 4.解构赋值</h5>
 
<a href="https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment">解构赋值</a>写法

<h5> 5.6个6问题 </h5>
如下代码，打印结果是？
```
let i;
for(i=0;i<6;i++){
  setTimeout(
    ()=>{
        console.log(i)},0
   )
 }
```
结果为6个6，由于i自始至终只有一个对象，所以虽然i的值一直在变化，但是执行log函数时，i的最终值为6。
这时候再去挨个执行log函数，所以最终的值为6个6。想打印0-5，两种方式：<br>

* 1.每次循环都声明一个对象，用来存储i的值：
```
            for (i = 0; i < 6; i++) {
                let j = i;
                setTimeout(() => {
                    console.log(j)
                }, 0)
            }
```
或者使用JS的语法糖：
* 2.直接在循环中声明let i:
```
            for (let i = 0; i < 6; i++) {
                setTimeout(() => {
                    console.log(i)
                }, 0)
            }
```
* 3.使用立即执行函数
```
            let i;
            for (i = 0; i < 6; i++) {
                !function(i){
                    setTimeout(()=>{
                        console.log(i)
                    },0)
                }(i);
            }
```
在函数调用的过程中，形参会复制实参，所以!function(i)中的i其实是循环中i的复制。