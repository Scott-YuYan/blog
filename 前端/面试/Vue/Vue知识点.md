##### 1.Vue实例中的date为什么限制使用function
 参考文章：<a href="http://gaocarri.top/post/vue%E5%85%B3%E4%BA%8Edata%E4%B8%BA%E4%BB%80%E4%B9%88%E6%98%AF%E5%87%BD%E6%95%B0%E8%BF%99%E4%BB%B6%E4%BA%8B/">vue关于data为什么是函数这件事</a>
 
 由于对象是内存地址的引用，使用对象的话，组件之间都会使用这个对象，也就是说，每个实例(组件)中data的数据是相互影响的。使用函数，
 每个组件都有一份data的拷贝，可以起到防止不同组件数据相互影响的作用。举例说明：
 ```
import My from "./components/My"

const vm = new Vue({
    render: h => {
        return h('div',[h(My),h(My)])
    },
});

// 组件My中的data为对象的情况，两个h(My)的data是共享的，其中一个做出修改另一个也会受影响
// 组件My中的data为函数的情况，Vue如果发现My是个函数的话，会执行My__date = My.data() 这时候My.data()是一个没有名字的全新的对象，
所以无论执行多少遍，每个My__data都是不一样的，也就不存在相互影响的问题。

```