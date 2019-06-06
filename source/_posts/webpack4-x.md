---
title: webpack4.x学习入门
date: 2018-12-20 10:08:29
tags: webpack
---

** 之前一直使用3.x，这次学学4.x，看看跟之前有什么不同的地方没 **
#### 什么是webpack？
> webpack是一个打包工具，他的宗旨是一切静态资源即可打包。有人就会问为什么要webpack？webpack是现代前端技术的基石，常规的开发方式，比如jquery,html,css静态网页开发已经落后了。现在是MVVM的时代，数据驱动界面。webpack将现代js开发中的各种新型有用的技术，集合打包。
#### 一、前端环境搭建
我们使用npm或yarn来安装webpack
```  
npm install webpack webpack-cli -g 
或者 
yarn global add webpack webpack-cli
```

在webpack3中，webpack本身和它的cli以前都是在同一个包中，第4版他们两者分开了,新建一个webpack的文件夹，webpack,初始化package.json并安装webpack-cli

```
cnpm init -y  //-y 默认所有的配置
cnpm i webpack webpack-cli --save-dev  //局部安装
```

#### 二、部署webpack 
在上面搭建好的环境项目中，我们来到package.json里配置我们的scripts
```
"scripts": { 
 "dist" : "node_modules/.bin/webpack -p"   //使用cnpm  run dist 就可以打包我们的项目了
},
```

#### 三、webpackp配置流程篇
我们在开发是一般会打包src下的什么文件呢？
*  发布时需要的html，css，js 
*  预编译器stylus，less，sass,es6的高级语法 
*  图片字体资源.png，.gif，.ico，.jpg 
*  文件间的require别名@等修饰符等等
*  webpack-dev-server以及跨域处理
*  jQuery插件全局使用  
通过这几点来讲解webpack中webpack.config.js的配置
首先在同目录下创建webpack.config.js文件，配置总览大概是这样
```
module.exports={
    //入口文件的配置项
    entry:{},
    //出口文件的配置项
    output:{},
    //模块：例如解读CSS,图片如何转换，压缩
    module:{},
    //插件，用于生产模版和各项功能
    plugins:[],
    //配置webpack开发服务功能
    devServer:{}
}
```
**entry**：配置入口文件的地址，可以是单一入口，也可以是多入口。 
**output**：配置出口文件的地址，在webpack2.X版本后，支持多出口配置。 
**module**：配置模块，主要是解析CSS和图片转换压缩等功能。 
**plugins**：配置插件，根据你的需要配置不同功能的插件。 
**devServer**：配置开发服务功能。

##### 首先是入口文件
```
entry: './index.js', 
//可配置多入口文件
entry:{
  'index' : './index.js',
  'demo'  : './demo.js',
}
```

##### output选项（出口配置）
```
//出口文件的配置项
output:{
    //打包的路径
    path:path.resolve(__dirname,'dist'),
    //打包后的引用路径
    publicPath: 'dist',
    //打包的文件名称
    filename:'bundle.js'
    
},
//多入口情况下
output:{
    //打包的路径
    path:path.resolve(__dirname,'dist'),
    //打包后的引用路径
    publicPath: 'dist',
    //打包的文件名称
    filename:'js/[name].js'    
},
//[name]的意思是根据入口文件的名称，打包成相同的名称，有几个入口文件，就可以打包出几个文件。
```

**直接这样写是不对的，需要在头部引入path**
`const path = require('path')`
其中path.resolve(__dirname,’dist’)就是获取了项目的绝对路径。
接下来在根目录下创建index.js，然后运行node_modules/.bin/webpack
如果你是全局安装，直接webpack即可，此条命令针对局部安装

##### 设置webpack-dev-server
要执行webpack-dev-server首先要使用npm下载
`cnpm install webpack-dev-server –-save-dev`
最简单的几项配置如下
```
devServer:{
  //设置基本目录结构
  contentBase:path.resolve(__dirname,'dist'),
  //服务器的IP地址，可以使用IP也可以使用localhost
  host:'localhost',
  //服务端压缩是否开启
  compress:true,
  //配置服务端口号
  port:1717
  proxy : [{
      context: ['/admin'],
      target: "http://mcljp.com",
      changeOrigin : true
  }]
}
```

**contentBase**:配置服务器基本运行路径，用于找到程序打包地址
**host**：服务运行地址，建议使用本机IP，这里为了讲解方便，所以用localhost
**compress**：服务器端压缩选型，一般设置为开启，如果你对服务器压缩感兴趣，可以自行学习
**port**：服务运行端口，建议不使用80，很容易被占用，这里使用了1717.
**proxy** : 本地代理，可进行跨域

开启热更新，你要是直接webpack-dev-server是不行的，需要在json文件下配置下scripts
```
"scripts": {
    "dev":"webpack-dev-server --open"
 },
//--open命令运行后自动打开浏览器进行浏览
```
配置好保存后，在终端里输入 npm  run  dev  就可以进行浏览了

#### 四、配置模块
>loaders是Webpack最重要的功能之一，通过使用不同的Loader，Webpack可以的脚本和工具，从而对不同的文件格式进行特定处理。
**test**：用于匹配处理文件的扩展名的表达式，这个选项是必须进行配置的
**use**：loader名称，就是你要使用模块的名称，这个选项也必须进行配置，否则报错
**include/exclude**:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）
**query**：为loaders提供额外的设置选项（可选）
##### 打包css文件
根目录下创建css文件，然后js文件require引入,npm安装style-loader，css-loader
`cnpm install style-loader css-loader --save-dev`

webpack.config.js的modules修改为下
```
module:{
  rules: [
      {
        test: /\.css$/,
        use: [ 'style-loader', 'css-loader' ]
      }
    ]
}
```

##### 打包HTML文件
打包html我们需要用上插件html-webpack-plugin，使用npm安装插件，并在头部引用
```
npm install --save-dev html-webpack-plugin
const htmlPlugin= require('html-webpack-plugin');
```

html-webpack-pugin基本属性如下
**title**: 用来生成页面的 title 元素filename: 输出的 HTML 文件名，默认是 index.html, 也可以直接配置带有子目录。 
**template**: 模板文件路径，比如‘./index.html’， 
**favicon**: 添加特定的 favicon 路径到输出的 HTML 文件中。
**minify**: {} | false , 传递 html-minifier 选项给 minify 输出hash: true | false, 如果为 true, 将添加一个唯一的 webpack 编译 hash 到所有包含的脚本和 CSS 文件，对于解除 cache 很有用。 
**cache**: true | false，如果为 true, 这是默认值，仅仅在文件修改之后才会发布文件。 
**showErrors**: true | false, 如果为 true, 这是默认值，错误信息会写入到 HTML 页面中chunks: 允许只添加某些块 (比如，仅仅 unit test 块) 
**chunksSortMode**: 允许控制块在添加到页面之前的排序方式，支持的值：'none' | 'default' | {function}-default:'auto' 
**excludeChunks**: 允许跳过某些块，(比如，跳过单元测试的块) 
**chunks**：chunks 默认会在生成的 html 文件中引用所有的 js 文件，当然你也可以指定引入哪些特定的文件。
**根目录下创建index.html**
```
plugins:[
  new htmlWebpackPlugin({
      template    : 'index.html',
      filename    : 'index.html',
      title       : title,
      inject      : true,
      hash        : true,
      chunks      : ['index']
  }),
]
```
终端进行打包，你就会发现html文件也被打包在我们的dist目录下了。

##### 图片，字体打包
要将图片和字体打包我们需要安装url-loader，首先进行安装
`npm install --save-dev  url-loader`
接下来进行配置
```
test: /\.(png|jpg|gif)/,
use: [{
        loader: 'url-loader',
        options: {
                limit: 5000,
                outputPath: 'resource/'
            }
 }]
```
**limit** : 文件大小小于limit参数，url-loader将会把文件转为DataURL
**outputPath** : 表示输出文件路径前缀。图片经过url-loader打包都会打包到指定的输出文件夹下。但是我们可以指定图片在输出文件夹下的路径。图片被打包时，就会在输出文件夹下新建（如果没有）一个名为resource的文件夹，把图片放到里面

##### css单独打包
安装后，config.js文件头部引用
```
npm install --save-dev extract-text-webpack-plugin
const extractTextPlugin = require("extract-text-webpack-plugin");
```
引入成功创建实例后需要在plugins属性中进行配置
```
const extractTextPlugin = new extractTextPlugin("/css/index.css")
{
      test: /\.css$/,
      use : extractTextPlugin.extract({
               fallback: 'style-loader',
               use: [
                  { loader: 'css-loader' },
               ]
      })
}
```
##### ES6转ES6
Babel的安装与配置
`cnpm install --save-dev babel-core babel-loader babel-preset-env babel-preset-react`
在webpack中配置Babel的方法如下
```
test:/\.(jsx|js)$/,
use:{
   loader:'babel-loader',
},
exclude:/node_modules/
```
需要在根目录下创建.babelrc文件
```
{
    "presets":["react","env"]
}
```
##### 打包第三方库，比如jQuery
使用plugin全局引用
ProvidePlugin是一个webpack自带的插件，Provide的意思就是装备、提供。因为ProvidePlugin是webpack自带的插件，所以要先再webpack.config.js中引入webpack
`const webpack = require('webpack')`
引入成功后配置我们的plugins模块
```
plugins:[
    new webpack.ProvidePlugin({
        $:"jquery"
    })
],
```
一些杂货铺插件，可用可不用
`new webpack.BannerPlugin('买菜的家朋版权所有')`
```
//抽离第三方库，单独打包，不易造成代码冗余
new webpack.optimize.CommonsChunkPlugin({
    //name对应入口文件中的名字，我们起的是jQuery
    name:['jquery'],
    //把文件打包到哪里，是一个路径
    filename:"assets/js/[name].js",
    //最小打包的文件模块数，这里直接写2就好
    minChunks:2
}),
```

#### 补充：目前发现与之前3.x有两处不同

1.webpack.optimize.CommonsChunkPlugin这个打包独立模块的插件已被抛弃更改为
```
optimization: { 
   splitChunks: {
   name: 'common',
   filename : 'js/base.js'
   } 
 }
```
2.css打包插件extractTextPlugin已不能进行webpack4.0的打包，需要更新
```
cnpm install --save-dev extract-text-webpack-plugin@next 
然后在打包就正常了

"devDependencies": {
    ...
    "extract-text-webpack-plugin": "^4.0.0-beta.0",
    ...
}
```


