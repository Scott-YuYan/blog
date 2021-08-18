#### 1.路由是什么？
"路由（routing）就是通过互联的网络把信息从源地址传输到目的地址的活动。路由发生在OSI网络参考模型中的第三层即网络层。"[维基百科]路由用于分发请求

表驱动编程：将路由与key值做一一映射，例如：
```
const routeTable = {
    '1':div1,
    '2':div2,
    '3':div3,
    ...
}
```

#### 2.hash模式？history模式？memory模式？

* 任何情况下都可以用hash做前端路由，但是这种模式的缺点在于SEO不友好("搜尋引擎最佳化(Search Engine Optimization，SEO)")

* 如果后端建所有的前端路由都渲染同一页面，那么可以用history模式(IE8以下不支持)

#### 3.Vue-Router源码