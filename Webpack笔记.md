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
  - `path`表示==所有==文件的输出路径
  - `filename`可以是路径`filename: 'work/main.js'`，生成的`main.js`文件会放在`path/work`目录下，而图片等其他资源则放在`path`目录下

- 加载器`loader`

  - webpack自身不能处理的资源借助`loader`、`Webpack`进行解析
  - 如果不写`eslint`、`babel`等`loader`的配置文件，则在`rules`对象中添加`options`对象写配置项

- 插件`plugins`

  - 扩展`Webpack`的功能。如`Eslint`

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
          filename: 'main.js', // 也可以指定目录
          // 清空上一次打包生成的内容
          clean: true,
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

- 不要使用`loader: 'css-loader'`，因为`loader`配置项只能使用一个加载器

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

- `.less`文件

  - 安装依赖：`npm i less-loader -D`

    ```js
    module.exports = {
        ...
        module: {
    		rules: [
    			{
    				test: /\.less/i,
    				use: [
              			'style-loader',
              			'css-loader',
                        'less-loader'
            		],
    			},
    		],
    	},
    }

## 处理静态资源

- 通用资源类型(asset)

  - webpack自身就可以处理，不需要加载器
  - 打包后会将相对路径引入`./img/xx.aa`的资源变成==url请求路径==
  - 文件命名`[hash]`可以添加规则，比如`[hash:10]`表示取哈希值前10位
  - 资源模块类型`type`
    - `asset/resource`发送一个单独的文件并导出 URL。之前通过使用 `file-loader` 实现
    - `asset/inline` 导出一个资源的 data URI。之前通过使用 `url-loader` 实现
    - `asset/source` 导出资源的源代码。之前通过使用 `raw-loader` 实现
    - `asset` 在导出一个 data URI 和发送一个单独的文件之间自动选择。之前通过使用 `url-loader`，并且配置资源体积限制实现

  ```js
  module.exports = {
      ...
      module: {
  		rules: [
  			{
  				test: /\.(png|jpe?g|gif|webp|svg)$/,
                  type: 'asset',
  				parser: {
                      // 将小于4kb的文件处理成base64格式
                      dataUrlCondition: {
                          maxSize: 4 * 1024 // 4KB
                      }
                  },
                  generator: {
                      // 既指定文件名命名规则，也指定文件分类存放路径
                      // hash表示哈希值为了文件不重名的唯一标识
                      // ext表示文件拓展名
                      // query表示如果静态资源地址是url且携带query查询参数则携带上
                      filename: 'img/[hash][ext][query]'
                  }
  			},
  		],
  	},
  }
  ```

## Eslint

- 为了规范代码格式，便于后续`Babel`做代码兼容性处理，再由`webpack`压缩代码

- 配置文件的写法：`.eslintrc`、`.eslintrc.js`、`.eslintrc.json`

- 配置

  ```js
  module.exports = {
      // 解析选项
      parserOptions: {
          ecmaVersion: 6, // ES语法版本
          sourceType: 'module', // ES模块化
          ecmaFeatures: { // ES其他特性
              jsx: true, // 如果是React项目，就需要开启jsx特性
          }
      },
      // 具体检查规则
      // off或0表示 关闭规则
      // warn或1表示 开启规则 且只提示warn
      // error或2表示 开启规则 且抛出异常error
      rules: {
          semi: 'error', // 是否禁止使用分号
          'array-callback-return': 'warn', // 数组方法的回调函数中有return语句
          // 一条规则有多个条件使用数组形式
          'default-case': [
              'warn', // switch语句中有default分支
              {commentPattern: '^no default$'} // 允许在最后注释no default就不执行规则
          ],
          eqeqeq: [
              'warn', // 是否强制使用===和!==
              'smart', // 除了少数情况不执行规则
          ]
      },
      // 继承其他规则
      // Eslint官方规则 eslint:recommended
      // Vue Cli官方规则 plugin:vue/essential
      // React Cli官方规则 react-app
      extends: ['plugin:vue/essential'],
      // 环境
      env: {
          node: true, // 启用node全局变量
          browser:true, // 启用浏览器中全局变量
      }
  }
  ```

- 使用

  - 安装：`npm i eslint-webpack-plugin eslint -D`

  - 在`webpack.config.js`中引入：`const eslint = require('eslint-webpack-plugin')`

    ```js
    module.exports = {
        ...
        plugins: [
            new eslint({
                // 需要检查的文件存放目录
                context: path.resolve(__dirname, 'js')
            })
        ]
    }
    ```

  - 创建`.eslintrc.js`文件，写入配置

  - 运行`npx webpack`就会检查并打包代码

  - 如果安装了`ESlint`插件，需要创建`.eslintignore`文件屏蔽`webpack`打包输出的目录

## Babel

- 用于将ES6语法编写的代码转换成向后兼容的JS语法，以便在旧版本浏览器上运行

- 配置文件的写法：`.babelrc`、`.babelrc.js`、`.babelrc.json`

- 配置

  ```js
  module.exports = {
      // 预设 一组babel插件拓展功能
      presets: [
          '@babel/preset', // 智能预设，允许使用最新的JS
          '@babel/preset-react', // 编译React jsx语法
          '@babel/preset-typescript', // 编译TS语法
      ]
  }
  ```

- 使用

  - 安装：`npm i babel-loader @babel/core @babel/preset-env -D`

  - 在`webpack.config.js`中使用

    - 排除指定==文件夹==下的文件`exclude: /正则/`

    ```js
    module.exports = {
        ...
        module: {
    		rules: [
                {
                    test: /\.js$/,
                    exclude: /node_modules/, // node_modules下的文件不需要打包编译
                    loader: 'babel-loader',
                }
    		],
    	},
    }
    ```

  - 创建`.babelrc.js`文件，写入配置

  - 运行`npx webpack`就会检查并打包代码