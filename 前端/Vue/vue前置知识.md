#### 1.Vue的历史
Vue读作vue，意为MVC中的V。
##### 1.1 Vue的版本
<ol>
<li>2013年，0.6、0.7版</li>
<li>2014年，0.8-1.1版</li>
<li>2015年，1.0版</li>
<li>2016年，2.0版</li>
<li>2019年，2.6版</li>
<li>2020年，3.0版</li>
</ol>
Vue的作者，尤雨溪，在创作了Vue的同时，还创作了Vue Router、Vuex、及@vue/cli

##### 1.2 Vue学习路线

<img width="712" alt="微信图片_20210629224616" src="https://user-images.githubusercontent.com/51253421/123820690-59e70480-d92d-11eb-8597-ffcf041955a0.png">

首先Vue文档，<a href="https://cn.vuejs.org/v2/guide/index.html">中文</a>,<a href="https://vuejs.org/v2/guide/index.html">英文</a>。

#### 2.Vue创建工具 @vue/cli
安装命令:`yarn global add @vue/cli` || `npm install -g @vue/cli`,安装完成后，新建目录，使用`vue create xxx(项目名)`创建项目，或者使用
`yarn create .`在当前目录创建项目,如果在git中出现，上下键无法选择的情况，那么使用`winpty vue.cmd create xxx`命令。配置选择如下：

![image](https://user-images.githubusercontent.com/51253421/123984987-adbe2000-d9f7-11eb-8c53-cbba362b3d4c.png)
命令执行完成后，进入目录，运行`yarn server`开启`webpack-dev-server`。


#### 3.Vue的不同版本
首先，相关的说明在<a href="https://cn.vuejs.org/v2/guide/installation.html">官网</a>上给出了：
<ul>
<li>完整版:包含编译器+运行时+注解</li>
<li>只包含运行时：运行时+注解</li>
<li>完整版(生产环境)：编译器+运行时</li>
<li>只包含运行时(生产环境)：运行时</li>
</ul>
举例说明完整版和运行时的区别：
完整版:

```
<body>
    <div id="num">
      {{n}}
      <button id="add" @click="add">+1</button>
    </div>
</body>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
```
创建Vue实例：
```
    el: '#num',
    data: {
        n: 0
    },
    methods: {
        add() {
            this.n += 1;
        }
    }
});
```
完整版加上了编译器，那么可以修改HTML中得到视图。

运行时:
 对于运行时，由于没有编译器，所以不能直接修改HTML中的内容，写法如下：
 ```
const num = new Vue({
    el: '#num',
//这里的date可以是对象或者函数
    data: {
        n: 10
    },
    methods: {
        add() {
            this.n += 1;
        }
    },
    render(createElement){
        const h=  createElement;
        return h('div',[this.n,h('button',{on:{click:this.add}},'+1')]);
    }
});
```
这种方式的方法在在于，在js中以createElement把所有的HTML元素构造出来，用js构造视图。<br>

以更容易理解的角度出发，完整版更容易让人接受，但是由于将HTML代码中的`@click`,`v-on`,`v-if`等指令编译成dom元素需要编译器完成，
所以这种方式更复杂，占用更多的代码体积。Vue也告诉我们："因为运行时版本相比完整版体积要小大约 30%，所以应该尽可能使用这个版本。"<br>
 
 对于即想使用不完整的Vue,节省体积，又希望直接对HTML的内容进行修改，如何实现呢？这就需要第三种方法，使用webpack中的vue-loader，
 新建.vue文件，执行`yarn build`命令后，调用编译器进行编译，这也是目前的最佳实践。
 总结：
 
 ![image](https://user-images.githubusercontent.com/51253421/124346733-0b7a8400-dc13-11eb-9377-b99bc14f5a6a.png)

 #### 4.新建.vue文件
 创建.vue文件，语法为：
 ```
<template></template>

<script></script>

<style></style>
```

