# webpack 核心概念

webpack基于nodejs开发的，所以书写语法符合commonJs规范。

## 核心概念：

loader、plugins、Entry、Output、SourceMap、DevServer、Hmr、Babel

**1.1 module**

```javascript
module.exports = { 
  module: {
    rules: [
      {
        test: /\.(jpg|png|gif)$/,
        use: {
          loader: "fole-loader",
          options: {
              name: "[name].[hash:5].[ext]", //占位符
              outputPath: "images", //基于前面配置的output路径
              limit: 2048 //小于该大小的图片会被处理成base64格式,直接放在被引入的文件中，以达到减少网络请求的目的;大于的才会被单独打包(生成单独的图片文件)
          }
        }
      },
        ...
    ]
  }
} 
```

1. file-loader

字体文件处理。

2. less-loader, css-loader

*.less格式的文件处理

style-loader 通过style标签将CSS直接注入到html页面

postcss-loader autoprefixer 样式的兼容性处理

use的执行顺序：从右到左、从下到上

3. url-loader

处理图片文件。

什么是loader？

文件预处理器

**1.2 plugins(插件)** -扩展webpack功能

1. html-webpack-plugin

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
	plugins: [
	  new HtmlWebpackPlugin({
      template: "./src/index.html"
    }),
    ...
	]
}
```



2. clean-webpack-plugin

每次打包前清除上次生成的文件。

**1.3 sourceMap** - 方便定位源码错误

```javascript
module.exports = {
	mode: "production|development",
 	//开发模式推荐 cheap-module-evel-source-map（会暴露源码）
  //生产环境下推荐 cheap-module-source-map(提示友好，不暴露源码)
 	devtool: "source-map"
}
```



**1.4 热更新** - HMR(hot module replacement)

开启步骤：

1. 使用webpack-dev-server作为服务器启动

2. devServer配置中 hot:true

3. plugins hotModuleReplacementPlugin

4. js模块中增加module.hot.accept增加HMR代码。

```javascript
//package.json
npm install webpack-dev-server
{
    "scripts": {
      "build": "webpack",
      "dev:server": "webpack-dev-server" //文件更改后，可以自动打包
    }
}
//webpack.config.js
module.exports = {
  mode: "production|development",
 	devServer: {
    //定义服务访问目录(在dist目录下启个服务)
	  contentBase: path.join(__dirname, "dist"),
	  port: 8081,
    hot: true
    //代理
	  //proxy: {
       //  "/api": "ip:port"
	  //}
	},
 	plugins: [
	  new webpack.HotModuleReplacementPlugin()
	]
}

//js文件
import {list} from './list'
if(module.hot){
  module.hot.accept("./list", () => {
    console.log('更新list文件模块');
   	list()
	})
 //关闭热更新
 module.hot.decline("./list")
}
```



**1.5 babel**

js的编译器

Babel转换注意事项：

- 标准引入的语法：箭头函数、let	、const等 可以转换

- 标准引入的全局变量、部分原生对象新增的原型链上的方法、Promise、Symbol、Set等 不可以转换。

  polyfill做兼容性处理

  @babel/polyfill 以全局变量的形式将方法注入，开发类库/UI组件时，造成全局变量的污染.

  替代方案：

  @babel/plugin-transform-runtime 以闭包方式注入，保证全局环境不被污染，推荐使用。

```javascript
npm install @babel/core @babel/preset-env babel-loader -D
npm install @babel/polyfill core-js@3 -D
npm install @babel/plugin-transform-runtime @babel/runtime-corejs3 --save-dev

//webpack.config.js
module: {
	rules: [
    {
      test: /\.js$/,
      loader: 'babel-loader',
      options: {
        // 转换ES5+语法
        //presets: [["@babel/preset-env", {
        //必须同时设置corejs:3  默认使用corejs:2
        //useBuiltIns: 'usage', //可选：usage|entry|false
        //corejs: 3
        //}]]
      },
      exclude: /node_modules/
	  }
	]
}

//.babelrc
{
	plugins: [
    [
      "@babel/plugin-transform-runtime",
      { corejs: 3 }
    ]
  ]	
}
```



## 高级概念：

TreeShaking、环境区分、CodeSpliting、打包分析、代码分割、环境变量使用

**2.1 TreeShaking**

依赖ES6模块语法。

```javascript
//loadsh-es lodash支持ES6语法的库
module.exports = {
    // webpack3 需要借助uglifyjsWebpackPlugin
    // webpack4 只需要设置为 “production” 即可
    mode: "production"
}
```



**2.2 dev prod区分**

开发环境：

devServer

sourceMap

接口代理、proxy

...

生产环境：

treeshaking

代码压缩

提供公共代码

共同点：

同样的入口

部分相同的代码处理

...

方案：

webpack.prod.js

webpack.dev.js

webpack.base.js  // 开发环境与生产环境共用的代码

借助一个工具：npm install webpack-merge

```javascript
// webpack.prod.js
const merge = require('webpack-merge')
const baseConfig = require('./webpack.base.js')
const prodConfig = {
    mode: "production", //只会开启treeshaking和js代码的压缩
    devtool: "none"
}
module.exports = merge(baseConfig, prodConfig)

//webpack.dev.js
const merge = require('webpack-merge')
const baseConfig = require('./webpack.base.js')

const devConfig = {
  mode: "development",
  devtool: "cheap-module-source-map",
  devServer: {
    //定义服务访问目录(在dist目录下启个服务)
    contentBase: path.join(__dirname, "dist"),
    port: 8081,
    //hot: true
    //代理
    //proxy: {
      //"/api": "ip:port"
    //}
	},
 	plugins: [
	  new webpack.HotModuleReplacementPlugin()
	]
}
module.exports = merge(baseConfig, devConfig)

//webpack.base.js

//package.json
{
    "scripts": {
      "build:prod": "webpack --config ./config/webpack.prod.js",
      "build:dev": "webpack-dev-server --config ./config/webpack.dev.jss",
      "dev:server": "webpack-dev-server --config ./config/webpack.dev.js" //文件更改后，可以自动打包
	}
}
```



**2.3 js打包优化**

1. 入口配置：entry多入口。      使用：wbpack.ProviderPlugin
2. 抽取公共代码：splitchunks(代码分割)。使用：splitChunksPlugins  之前的版本：commonChunksPlugin :banana:
3. 动态加载：按需加载，懒加载。 使用：@babel/plugin-syntax-dynamic-import

```javascript
//多入口配置案例
//import $ from 'jquery'
//做了如下配置后，如果代码中需要使用jquery，则不需要做如上代码的引入，直接使用即可。
//webpack.base.config
import  webpack foom 'webpack'
entry: {
    index:"./src/index.js",
    jquery:  "jquery"
}
plugins: [
    new wbpack.ProviderPlugin({
        $: 'jquery',
        jQuery: 'jquery'
    })
]

//动态加载案例
npm install -D @babel/plugin-syntax-dynamic-import

//index.js
import(/*webpackChunkName:'jquery'*/'jquery').then((default:$) => {
  console.log($);
})

//.babelrc
{
  plugins: [
    "@babel/plugin-syntax-dynamic-import",
	  [
	    "@babel/plugin-transform-runtime",
      { corejs: 3 }
	  ]
	]	
}

//抽取公共代码
//webpack.base.js
module.exports = {
  optimization: {
    splitChunks: {
      chunk: 'all' // initial|async|all
    }
  }
}
```

**2.4 css文件代码分割**

配置代码分割压缩css代码，optimization选项需要注意⚠️：影响到js代码的压缩，需要手动对其进行配置，使用`terser-webpack-plugin`

```javascript
npm install -D mini-css-extract-plugin
//css代码压缩
npm install -D optimize-css-assets-webpack-plugin 
//webpack.prod.js
const MiniCssExtractPlugin = require('mini-css-extract-plugin') //css提取
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin'); //css压缩
const TerserPlugin = require('terser-webpack-plugin'); //js压缩

const prodConfig = {
  ...
  optimization: {
    minimizer: [new OptimizeCssAssetsPlugin({}), new TerserPlugin()] //配置css的压缩后会覆盖掉js的压缩，所以需要再配置js的压缩插件
  },
  plugins: [
    new MiniCssExtractPlugin({
    	filename: "[name].[hash:5].css"
 	 	})
  ],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {}
          },
          {
            loader: 'css-loader'
          }
        ],
      },
    ],
  },
}
```



**2.5 代码包分析工具**

```sh
npm install --save-dev webpack-bundle-analyzer
```



**2.6 获取环境参数**

webpack设置环境变量后，使用yargs插件获取参数值。

```shell
npm install yargs
```

