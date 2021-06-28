执行：`yarn add vue`后即可使用`import Vue from 'vue'`即可，如果使用Vue的过程中控制台出现报错，那么可以通过在`package.json`中
增加
```
"alias": {
     "vue": "./node_modules/vue/dist/vue.common.js"
   }
```