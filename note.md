# webpack

## 介绍

- webpack 是一种前端资源构建工具，一个静态模块打包器(module bundler)。在 webpack 看来, 前端的所有资源文件(js/json/css/img/less/...)都会作为模块处理。 webpack 将根据模块的依赖关系进行静态分析，打包生成对应的静态资源(bundle)。
- 首先告诉 webpack 打包起点（入口文件），记录每一个依赖，形成依赖关系图，按照依赖图将 js/css 等文件引入形成 chunk(代码块)并进行各项处理（将 less 编译成 css, 将 es6 编译成 es5 等），将打包好的资源进行输出(bundle)。

## webpack 五个核心概念

### Entry

入口(Entry)指示 webpack 以哪个文件为入口起点开始打包，分析构建内部依赖图。

### Output

输出(Output)指示 webpack 打包后的资源 bundles 输出到哪里去，以及如何命名。

### Loader

将不同类型的文件转换为 webpack 可识别的模块。Loader 让 webpack 能够去处理那些非 JavaScript 文件 (webpack 自身只理解 JavaScript)

### Plugins

插件(Plugins)可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩， 一直到重新定义环境中的变量等。

### Mode

模式(Mode)指示 webpack 使用相应模式的配置。

- development（能让代码本地调试 运行的环境）：会将 DefinePlugin 中 process.env.NODE_ENV 的值设置 为 development。启用 NamedChunksPlugin 和 NamedModulesPlugin。
- production（能让代码优化上线 运行的环境）：会将 DefinePlugin 中 process.env.NODE_ENV 的值设置 为 production。启用 FlagDependencyUsagePlugin, FlagIncludedChunksPlugin, ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, OccurrenceOrderPlugin, SideEffectsFlagPlugin 和 TerserPlugin。

## 运行指令：

- 开发环境：webpack ./src/index.js -o ./build/built.js --mode=development
      webpack会以 ./src/index.js 为入口文件开始打包，打包后输出到 ./build/built.js
      整体打包环境，是开发环境
- 生产环境：webpack ./src/index.js -o ./build/built.js --mode=production
      webpack会以 ./src/index.js 为入口文件开始打包，打包后输出到 ./build/built.js
      整体打包环境，是生产环境
- 结论
    1. webpack能处理js/json资源，不能处理css/img等其他资源
    2. 生产环境和开发环境将ES6模块化编译成浏览器能识别的模块化
    3. 生产环境比开发环境多一个压缩js代码。

## 使用

### webpack.config.js - webpack的配置文件

- 作用: 指示 webpack 干哪些活（当运行 webpack 指令时，会加载里面的配置）
- 所有构建工具都是基于nodejs平台运行的~模块化默认采用commonjs。

```js
// resolve用来拼接绝对路径的方法
const { resolve } = require('path');
module.exports = {
  // webpack配置
  // 入口起点
  entry: './src/index.js',
  // 输出
  output: {
    // 输出文件名
    filename: 'built.js',
    // 输出路径
    // __dirname是nodejs的变量，代表当前文件(webpack.config.js)的目录绝对路径
    path: resolve(__dirname, 'build')
  },
  // loader的配置
  module: {
    rules: [
      // 详细loader配置
      // 不同文件必须配置不同loader处理
      {
        // 匹配哪些文件
        test: /\.css$/,
        // 使用哪些loader进行处理
        use: [
          // use数组中loader执行顺序：从右到左，从下到上 依次执行
          // 创建style标签，将js中的样式资源插入进行，添加到head中生效
          'style-loader',
          // 将css文件变成commonjs模块加载js中，里面内容是样式字符串
          'css-loader'
        ]
      },
      {
        test: /\.less$/,
        use: [
          'style-loader',
          'css-loader',
          // 将less文件编译成css文件
          // 需要下载 less-loader和less
          'less-loader'
        ]
      }
    ]
  },
  // plugins的配置
  plugins: [
    // 详细plugins的配置
  ],
  // 模式
  mode: 'development', // 开发模式
  // mode: 'production' //生产模式
}
```
