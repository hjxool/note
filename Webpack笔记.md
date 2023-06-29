## 基本使用

- ==静态资源==打包工具
- Webpack本身功能只有：编译及压缩JS中的`ES Module`语法
- 以一个或多个文件作为打包入口，将项目所有文件编译==合成==一个或多个文件输出
- 浏览器不能识别ES6模块化语法，除非`<script type="module" src="./main.js">`，为了能够更方便的使用`<script src="./main.js">`，就需要Webpack打包工具
- 安装：`npm i webpack webpack-cli -D`
  - 因为项目上线后不需要开发阶段的工具包，只需要项目能够正常运行的依赖包即可

- 打包：`npx webpack ./main.js --mode=development(开发)或production(生产)`
  - 开发环境
    - **仅**会对`import`、`export`进行编译
  - 生产环境
    - 除了对ES6模块化语法进行编译，还会对ES6箭头函数等方法进行编译，将整体代码进行==压缩==
  - 基本功能下只能处理`js`资源
  - 有了`webpack.config.js`配置文件后，在`webpack.config.js`所在层级使用命令行`npx webpack`运行即可

## 配置

- 入口`entry`

  - 指示webpack从哪个文件开始打包

- 输出`output`

  - 指示webpack打包完的文件输出到何处，如何命名等

- 加载器`loader`

  - webpack自身不能处理的资源借助`loader`、`Webpack`进行解析

- 插件`plugins`

  - 扩展`Webpack`的功能

- 模式`mode`

  - 开发者模式：`development`
  - 生产模式：`production`

- 在==项目根目录==下创建`webpack.config.js`文件

  ```js
  const path = require('path')
  // 必须使用nodeJS的模块化语法 毕竟压缩的是ES语法，不可能再用ES模块化语法
  module.exports = {
      // 入口 必须用 相对路径
      entry: './main.js',
      // 输出
      output: {
          // 文件的输出路径 必须用 绝对路径
          path: path.resolve(__dirname,'dist'),
          // 文件名
          filename: 'main.js'
      },
      // 加载器
      module: {
  		// loader的配置
  		rules: [],
  	},
      // 插件 数组
      plugins: [],
      // 模式
      mode: 'development'
  }

## 处理样式资源

- `.css`文件

  - 安装依赖：`npm i css-loader style-loader -D`
  - 想要打包资源，必须引入该资源

  ```js
  module.exports = {
      ...
      module: {
  		rules: [
  			{
  				test: /\.css/i, // 只检测.css(忽略大小写)文件
  				use: [ // use执行顺序从数组末尾到开头
            			'style-loader', // 将编译后的css模块以<style>形式加入到HTML文件中
            			'css-loader' // 将css资源编译成nodeJS模块导入到js中
          		],
  			},
  		],
  	},
  }
  ```

- `.less`文件