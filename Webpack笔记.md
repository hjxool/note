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
- 有了配置文件后
  - 打包：`npx webpack`
  - 开发时不打包只在内存中编译：`npx webpack serve`
  - 当配置文件不是`webpack.config.js`时
    - `npx webpack serve(可选) --config ./folder/webpack.dev.js`
      - `--config`表示指定配置文件路径，且应用配置文件可以==自定义名称==

- 在`package.json`文件中，配置项`scripts`下像`dev`等运行指令`webpack --config ./webpack.dev.js`不需要`npx`，因为这里`webpack `等指令都是全局的
  - 可以理解为`scripts`下的配置`'xxx': 'webpack'`，在命令行运行`npm run xxx`等同于`npx webpack`


## 配置

- 入口`entry`

  - 指示webpack从哪个文件开始打包

- 输出`output`

  - 指示webpack打包完的文件输出到何处，如何命名等
  - `path`表示==所有==文件的输出路径
    - 开发时不需要打包输出，`path`可以为`undefined`
  - `filename`可以是路径`filename: 'work/main.js'`，生成的`main.js`文件会放在`path/work`目录下，而图片等其他资源则放在`path`目录下

- 加载器`loader`

  - webpack自身不能处理的资源借助`loader`、`Webpack`进行解析
  - 如果不写`eslint`、`babel`等`loader`的配置文件，则在`rules`对象中添加`options`对象写配置项

- 插件`plugins`

  - 扩展`Webpack`的功能。如`Eslint`
  - 都需要在`webpack.config.js`引入
  - 都有`xxx.config.js`配置文件

- 模式`mode`

  - 开发者模式：`development`
  - 生产模式：`production`

- 在==项目根目录==下创建`webpack.config.js`文件

  ```js
  const path = require('path')
  // 复用方法
  fn(...params){
      return [...]
  }
  
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
  		rules: [
              {  // use写法1
                 use: ['xxx1','xxx2'],
                 // use写法2——内嵌配置项时
                 use: ['xxx1',{ loader: 'xxx2',options:{} }]
                 // use写法3——多个重复内容复用
                 use: fn(params)
              }
          ],
  	},
      // 插件 数组
      plugins: [],
      // 模式
      mode: 'development'
  }

## 动态插入样式

- 不要使用`loader: 'css-loader'`，因为`loader`配置项只能使用一个加载器

- `.css`文件

  - 安装依赖：`npm i css-loader style-loader -D`

    - `style-loader`会动态创建`<style>`标签

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
    ```

## 单独插入样式

- 默认打包方式，是将样式文件打包到js文件中，浏览器运行打包的文件会==先解析HTML结构==，再加载js文件，==在js文件运行时==，才会创建`<style>`标签生成样式，这样会导致==闪屏==，因此利用`plugin`插件将样式文件单独插入`<link>`标签性能更好，这样页面加载流程就变成`加载样式文件——解析html结构——加载js文件——运行js文件`

- 使用

  - 安装：`npm i mini-css-extract-plugin -D`

  - 在`webpack.config.js`中引入：`const css = require('mini-css-extract-plugin')`

    ```js
    module.exports = {
        ...
        plugins: [
            new css({
                // 指定存放目录和文件名
                filename: 'css/main.css'
            })
        ]
    }

  - 因为`style-loader`会动态创建`<style>`标签，因此所有用到`use:['style-loader']`要替换成`use:[css.loader]`

    - 注：`css.loader`不是字符串，是对象方法

- Tips

  - 打包后会将所有样式集合成一个`.css`文件，并插入打包生成的`.html`文件中

## CSS兼容性处理

- 配置`postcss.config.js`文件

  ```js
  module.exports = {
    plugins: [
      [
        'postcss-preset-env',
        {
          // 其他选项
        },
      ],
    ],
  };

- 使用

  - 安装：`npm i postcss-loader postcss postcss-preset-env -D`

  - 在`webpack.config.js`中==插入==所有css相关加载器

    ```js
    module.exports = {
        ...
        module: {
    		rules: [
    			{	// 以less-loader为例
    				test: /\.less$/,
    				use: [
                        css.loader, // mini-css-extract-plugin插件方法
                        'css-loader',
                        // postcss-loader接收less-loader处理结果
                        // 在处理兼容性问题后交给css-loader
                        'postcss-loader',
                        'less-loader',
                    ],
    			},
    		],
    	},
    }
    ```

  - 在`package.json`里配置兼容条件

    - 如`'browserslist': ['ie >= 8']`，表示兼容性支持IE8以上
    - 常用：`browserslist': ['last 2 version', '> 1%', not dead]`
      - `last 2 version`只要最近的两个版本
      - `> 1%`覆盖99%的浏览器
      - `not dead`已经废弃的浏览器不支持

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
      },
      // 开启缓存
      cache: true,
      cacheLocation: path.resolve(__dirname, './node_modules') // 指定缓存文件路径
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
                context: path.resolve(__dirname, 'js'),
                // 也可以用exclude/include
                exclude: /node_modules/
            })
        ]
    }
    ```

  - 创建`.eslintrc.js`文件，写入配置

  - 运行`npx webpack`就会检查并打包代码
  
  - 如果安装了`ESlint`插件，需要创建`.eslintignore`文件屏蔽`webpack`打包输出的目录

## Babel

- 用于将ES6语法编写的代码转换成向后兼容的JS语法，以便在旧版本浏览器上运行

- 配置文件的写法：`.babelrc`、`.babelrc.js`、`.babelrc.json`、`babel.config.js`

- 配置

  ```js
  module.exports = {
      // 预设 一组babel插件拓展功能
      presets: [
          '@babel/preset', // 智能预设，允许使用最新的JS
          '@babel/preset-react', // 编译React jsx语法
          '@babel/preset-typescript', // 编译TS语法
      ],
      // 开启缓存 Eslint插件也可用
      cacheDirectory: true, // 开启babel缓存
      cacheCompression: false, // 关闭缓存文件压缩
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
                    // 或者 只能写一个
                    include: path.resolve(__dirname, './js') //只处理指定目录下的js文件
                    loader: 'babel-loader',
                }
    		],
    	},
    }
    ```

  - 创建`.babelrc.js`文件，写入配置
  
  - 运行`npx webpack`就会检查并打包代码

## 自动引入打包资源

- 手动引入资源打包会比较繁琐，有的文件还涉及依赖关系就会比较麻烦，因此使用`plugin 插件`来自动化导入

- 使用

  - 安装：`npm i html-webpack-plugin -D`

  - 在`webpack.config.js`中引入：`const html = require('html-webpack-plugin')`

    ```js
    module.exports = {
        ...
        plugins: [
            new html({
                // 以index.html文件创建新的html文件 不然打包后html结构丢失
                template: path.resolve(__dirname, '/index.html').
                // 指定目录及文件名
                filename: 'html/index.html'
            })
        ]
    }
    ```

## 自动化打包

- 使用

  - 安装：`npm i webpack-dev-server -D`

  - 在`webpack.config.js`中配置

    - 注：不会输出打包文件，是在内存中编译打包的
  
    ```js
    module.exports = {
        ...
        // 开发服务器
        devServer: {
            host: 'localhost', // 启动服务器域名
            port: '30', // 启动服务器端口号
            open: true, // 是否自动打开浏览器
        }
    }
    ```
  
  - 运行：`npx webpack serve`

## CSS压缩

- 使用

  - 安装：`npm i css-minimizer-webpack-plugin -D`

  - 在`webpack.config.js`中引入：`const cssMini = require('css-minimizer-webpack-plugin')`

    ```js
    module.exports = {
        ...
        plugins: [
            new cssMini()
        ]
    }

## SourceMap源代码映射

- 通常会遇到这样一个问题：打包后的代码如果报错，指向的是编译后的压缩代码，很难断点调试，这就需要工具将==源代码==和==打包后代码==映射关联

- `SourceMap`就是这样一个映射文件方案，它会生成一个`xxx.map`文件，里面包含源代码和构建代码间映射关系，当构建代码出错，会通过`xxx.map`文件，让浏览器提示源代码出错位置

- 使用

  - 开发模式：`cheap-module-source-map`

    - 优点：打包速度快，只包含行映射

    - 缺点：没有列映射

    - 开发模式下打包的JS代码不会被压缩，所以定位到行即可

    - `webpack.config.js`文件

      ```js
      module.exports = {
          ...
          mode: 'development',
          devtool: 'cheap-module-source-map'
      }

  - 生产模式：`source-map`

    - 优点：包含行/列映射

    - 缺点：打包速度慢

    - 生产模式下打包的JS代码会会被压缩成一行，只靠列无法映射对应的位置

    - `webpack.config.js`文件

      ```js
      module.exports = {
          ...
          mode: 'production',
          devtool: 'source-map'
      }

## 提升打包速度

- `HotModuleReplacement`

  - 开发中修改某一处代码，就得重新打包，这个过程耗时较久

  - `HotModuleReplacement`可以做到只对==进行修改的模块==重新打包，在==程序运行中==，替换、添加、删除某个模块，而无需加载整个页面

    ```js
    module.exports = {
        ...
        // webpack-dev-server默认配置
        devServer: {
            ...
            hot: true, // 默认为true
        }
    }
    ```

  - 但是重新加载JS代码还是会让整个页面刷新，因此需要手动写入规则

    ```js
    // main.js
    import module1 from './js/module1'
    // 检测是否支持HotModuleReplacement功能
    if(module.hot){
        module.hot.accept('./js/module1') // module1发生变化只加载对应文件
        ...
    }

  - `vue-loader`、`react-hot-loader`都支持这个功能

- `OneOf`

  - 打包时会对每一个文件挨个比对`加载器`中每一个规则，效率很低

  - 使用

    ```js
    module.exports = {
        ...
        module: {
    		rules: [
    			{	// 用oneOf数组包裹原先rules中的规则
    				oneOf: [
                        {test:/\.less/i, use:[...]},
                        {test:/\.js$/, use:[...]}
                    ]
    			},
    		],
    	},
    }

- `Include/Exclude`
  - 开发时使用的第三方库，如`node_modules`等不需要打包
  - `include`和`exclude`配置项只能写一个
  - 用法见`Babel`章节
- `Cache`
  - 每次打包，JS文件都会经过`Eslint`、`Babel`编译，速度较慢。使用`cache`缓存之前的检查编译结果，只对修改的文件重新检查，再次打包速度就会更快
  - 用法见`Babel`、`Eslint`章节
- `Thead`
  - 多==进程==打包==JS代码==
    - 注：仅适用特别耗时的操作，因为每个进程都有启动时间，大约`600ms`
  - 使用
    - 安装：`npm i `