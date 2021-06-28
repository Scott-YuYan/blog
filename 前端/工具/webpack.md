#### 1.安装

去<a href="https://webpack.js.org/guides/getting-started/">官网</a>搜索,安装命令为`npm install webpack webpack-cli --save-dev`
同时安装webpack及webpack-cli(使用yarn的话为`yarn add webpack webpack-cli --dev`)。除此之外，还需要安装webpack-dev-server用于预览,
查看帮助文档，可使用`npx webpack`命令。

#### 2.作用
webpack主要用于:1.转译代码(ES6转为ES5,SCSS转为CSS) 2.构建项目 3.代码压缩 4.代码分析

##### 2.1 使用前准备
根据官网的提示，使用`yarn init -y`命令生成`package.json`文件,并使用前面的安装命令，安装程序包。命令执行完成后，可以在项目中看到
`node_modules`文件夹，可在项目根目录下使用` ./node_modules/.bin/webpack --version` || `npx webpack --version`命令查看安装是否完成。

##### 2.2用webpack转译JS
使用`npx webpack` || `./node_modules/.bin/webpack` 对js进行转译,运行这一步可以让`webpack`将我们的代码转译为简单的js,使得
低版本的浏览器也可以运行。这时候一般控制台都会给警告，让我们设置mode,代码如下：
```
module.exports = {
    mode: "development"(开发模式) || "production"(生产模式),
};
```
与此同时，还可以增加配置：
```
 entry: './foo.js',
  output: {
    path: path.resolve(__dirname, 'dist'),//默认值，可省略
    filename: 'foo.bundle.js',
  },
```
指明入口文件以及导出文件名称

##### 2.3 使用webpack生成html
<a href="https://github.com/jantimon/html-webpack-plugin#options">html-webpack插件</a>

##### 2.4 使用webpack引入css
需要用到<a href="https://webpack.js.org/loaders/css-loader/">css-loader插件</a>

##### 2.5 使用webpack-dev-server自动刷新
用到<a href="https://webpack.js.org/guides/development/">webpack-dev-server插件</a>，在package.json中增加
`"start": "webpack serve --open"`，可以将`-- open`去掉,即可关闭自动打开浏览器。这样通过`yarn start`启动，对代码做出的修改
即可自动刷新。需要注意的是，HTML页面是在内存中生成的，所以dist目录中不会有内容。

##### 2.6 使用mini-css-extract-plugin抽取css文件
前面使用css-loader插件会将css写入到HTML中，如果想单独将css抽取成文件，可以使用<a href="https://webpack.js.org/plugins/mini-css-extract-plugin/">mini-css-extract插件</a>
，执行`yarn build`后，即可看到css被单独抽取成文件,即便是多个css文件，也会被抽取成一个css文件。

##### 2.7 不同的环境使用不同的配置
例如，对于生产环境所使用的配置，可新建配置文件`webpack.config.prod.js`,在`package.json`中，使用
`   "build": "rm -rf dist && webpack --config webpack.config.prod.js",`声明,`yarn build`命令将会使用生产环境的js文件。

##### 2.8 使用webpack引入scss
使用<a href="https://webpack.js.org/loaders/sass-loader/">sass-loader加载器</a>,但是需要注意的是,`node-sass`已经过时了推荐使用`dart-sass`,根据官网给出的代码：
```
module.exports = {
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: [
          // Creates `style` nodes from JS strings
          "style-loader",
          // Translates CSS into CommonJS
          "css-loader",
          // Compiles Sass to CSS
          "sass-loader",
        ],
      },
    ],
  },
};
```
新增rules,这时候执行`yarn build`命令可能会报错,根据github的<a href="https://github.com/webpack-contrib/sass-loader/issues/435">issue</a>,将`sass-loader`修改为
```
{
   loader: 'sass-loader',
    options: {
     implementation: require('dart-sass'),
    },
```

##### 2.9 使用webpack引入LESS和Stylus
less与scss一样，都是css的一门变种语言，也都支持css。在webpack中使用使用<a href="https://webpack.js.org/loaders/less-loader/">less-loader加载less文件</a>,并增加rules:
```
{
        test: /\.less$/i,
        loader: [
          // compiles Less to CSS
          "style-loader",
          "css-loader",
          "less-loader",
        ],
      },
```
对于Stylus,使用<a href=""https://webpack.js.org/loaders/stylus-loader/>stylus-loader</a>,并增加rules:
```
 {
        test: /\.styl$/,
        loader: [
            "style-loader",
            "css-loader",
            "stylus-loader",
            ] // compiles Styl to CSS
      },
```

##### 2.10 使用webpack引入图片
使用<a href="https://v4.webpack.js.org/loaders/file-loader/">file-loader</a>并粘贴rules,那么即可直接在js中import图片：
```
import qq from './static/qq.png'
```
以及获取该图片：
```
app.innerHTML = `
<img src="${qq}">
`;
```

#### 2.11 使用import实现懒加载
```
button.onclick = ()=>{
    const promise = import('./lazy'); //lazy为js导出对象
    promise.then((module)=>{
    const fn = module.default;
    fn();
},()=>{
    console.log('模块加载错误');
});
}
```

#### 3.webpack一键部署到github

##### 3.1 yarn build 后，dist目录下的html文件用http-server预览，用parcel会报错。


#### .webpack面试题
 ##### 4.1 webpack loader与webpack plugin 的区别？
 loader为加载器，用于加载文件，例如label-loader用于将高级的js加载成ie支持的js、css/style-loader用于加载css文件，将其加载为页
 面中的style标签。插件则是用于加强功能，例如HtmlWebpackPlugin，MiniCssExtractPlugin
