## 基本使用

- ==静态资源==打包工具

- 想要打包资源，**必须**引入该资源

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

- 在`package.json`文件中，配置项`scripts`下像`dev`等运行指令`webpack --config ./webpack.dev.js`不需要`npx`，因为这里`webpack`等指令都是全局的
  - 可以理解为`scripts`下的配置`'xxx': 'webpack'`，在命令行运行`npm run xxx`等同于`npx webpack`

- 如果用`loader`就可以把`options`写同级

  ```js
  module: {
      rules: [
        {
          loader: 'xxx-loader'，
  　　　　　options: {...}
        },
        // 如果用use options就只能写到use里
        {
            use: [{
                loader: 'xxx-loader',
                options: {...}
            }, 'xxx2-loader']
        }
      ]  
  }
  ```

- (不确定)入口`entry`路径不能是`../`开头，可以用`path.resolve(__dirname, '../src/xxx.js')`

- `[name]`是以entry属性名命名所有带`[name]`的文件


## 配置

- 入口`entry`

  - 指示webpack从哪个文件开始打包

- 输出`output`

  - 指示webpack打包完的文件输出到何处，如何命名等
  - `path`表示==所有==文件的输出路径
    - 开发时不需要打包输出，`path`可以为`undefined`
  - `filename`可以是路径`filename: 'work/main.js'`，生成的`main.js`文件会放在`path/work`目录下，而图片等其他资源则放在`path`目录下

- 加载器`module`

  - webpack自身不能处理的资源借助`loader`、`Webpack`进行解析
  - 如果不写`eslint`、`babel`等`loader`的配置文件，则在`rules`对象中添加`options`对象写配置项

- 插件`plugins`

  - 扩展`Webpack`的功能。如`Eslint`
  - 都需要在`webpack.config.js`引入
  - 都有`xxx.config.js`配置文件

- 模式`mode`

  - 开发者模式：`development`
  - 生产模式：`production`

- 优化`optimization`

  - 压缩`minimizer`、代码分割`splitChunks`的配置

- ==开发==服务器(自动打包)`devServer`

  - 方便开发时保存自动打包，且是在内存中编译，读写速度快

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
                 use: ['xxx1',{ loader: 'xxx2',options:{} }],
                 // use写法3——多个重复内容复用
                 use: fn(params),
              }
          ],
  	},
      // 插件 数组
      plugins: [
          new html({
              template: path.resolve(__dirname, '/index.html'),
              filename: 'html/index.html'
          })
      ],
      // 模式
      mode: 'development',
      // 优化配置
      optimization: { 
          minimizer: [// 压缩
              new cssMini()
          ],
          splitChunks: {// 代码分割
              chunks: 'all',
          }
      },
      // 开发服务器
      devServer: {
          host: 'localhost', // 启动服务器域名
          port: '30', // 启动服务器端口号
          open: true, // 是否自动打开浏览器
      }
  }

## 开发/生产配置文件

- 生产和开发流程略有不同，如开发打包时不需要生成文件来减少打包速度

- 开发模式

  - 配置文件`xx\configs\webpack.dev.js`

    ```js
    // 引入插件相同 略过
    module.exports = {
    	mode: 'development',
    	entry: path.resolve(__dirname, '../src/main.ts'),
    	// output不能少
    	output: {
    		path: undefined, // 没有路径 因为是在内存编译
    		filename: 'js/[name].js', // 文件名还是需要指定
    	},
    	// 其他配置相同
        devtool: 'cheap-module-source-map',
        devServer: {
            host: 'localhost', // 启动服务器域名
            port: '30', // 启动服务器端口号
            open: true, // 是否自动打开浏览器
        }
    };
    ```

  - 修改配置文件`根目录\package.json`

    ```json
    {
        "scripts": {
    		"server": "webpack serve --config ./xx/configs/webpack.dev.js",
            ...
    	},
    }

- 生产模式

  - 配置文件`xx\configs\webpack.pro.js`

    ```js
    const path = require('path');
    const css = require('mini-css-extract-plugin');
    const html = require('html-webpack-plugin');
    
    module.exports = {
    	mode: 'production',
    	entry: path.resolve(__dirname, '../src/main.ts'), //入口是ts
    	output: {
    		path: path.resolve(__dirname, '../bundle'),
    		filename: 'js/[name].js', //生成浏览器可识别的js文件
    		clean: true,
    	},
    	module: {
    		rules: [
    			{
    				oneOf: [
    					{
    						test: /\.css$/,
    						use: [css.loader, 'css-loader'],
    						include: path.resolve(__dirname, '../src'),
    					},
    					{
    						test: /\.ts$/,
    						use: ['ts-loader'],
    						include: path.resolve(__dirname, '../src'),
    					},
    				],
    			},
    		],
    	},
    	plugins: [
    		new css({
    			filename: 'css/index.css',
    		}),
    		new html({
    			template: path.resolve(__dirname, '../src/index.html'),
    			filename: 'html/index.html',
    		}),
    	],
    	devtool: 'source-map',
    };

  - 修改配置文件`根目录\package.json`

    ```json
    {
        "scripts": {
    		"build": "webpack --config ./xx/configs/webpack.pro.js",
            ...
    	},
    }

## 动态插入样式

- 不要使用`loader: 'css-loader'`，因为`loader`配置项只能使用一个加载器

- `.css`文件

  - 安装依赖：`npm i css-loader style-loader -D`

    - `style-loader`会动态创建`<style>`标签

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
                // 如果有多入口就需要用规则命名
                filename: 'css/[name].css',
                chunkFilename: 'css/[name].chunk.css'
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
      plugins: ['import'],// 不同版本eslint有可能不支持动态导入import函数
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

## HTML自动引入打包资源

- 手动引入资源打包会比较繁琐，有的文件还涉及依赖关系就会比较麻烦，因此使用`plugin 插件`来自动化导入，这样就不需要管打包生成的==JS文件==在哪个目录，叫什么，打包后生成的html文件就会自动引入

- 使用

  - 安装：`npm i html-webpack-plugin -D`

  - 在`webpack.config.js`中引入：`const html = require('html-webpack-plugin')`

    ```js
    module.exports = {
        ...
        plugins: [
            new html({
                // 以index.html文件创建新的html文件 不然打包后html结构丢失
                template: path.resolve(__dirname, '/index.html'),
                // 指定目录及文件名
                filename: 'html/index.html'
            })
        ]
    }
    ```

## 自动打包(开发服务器)

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
        optimization: {
            minimizer: [
                new cssMini()
            ]
        }
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

### HotModuleReplacement

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

### OneOf

- 打包时会对每一个文件挨个比对`加载器`中每一个规则，效率很低，`oneOf`使得文件在匹配到==第一个==与之对应的`loader`后就结束遍历

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

### Include/Exclude

- 开发时使用的第三方库，如`node_modules`等不需要打包
- `include`和`exclude`配置项只能写一个
- 用法见`Babel`章节

### Cache

- 每次打包，JS文件都会经过`Eslint`、`Babel`编译，速度较慢。使用`cache`缓存之前的检查编译结果，只对修改的文件重新检查，再次打包速度就会更快
- 用法见`Babel`、`Eslint`章节

### Thead

- 多==进程==打包==JS代码==
  - 注：仅适用特别耗时的操作，因为每个进程都有启动时间，大约`600ms`
- 使用
  - 安装：`npm i thread-loader -D`

  - 在`webpack.config.js`中

    ```js
    const os = require('os') // 引入nodeJS核心模块
    const threads = os.cpus().length // cpu核数
    const terser = require('terser-webpack-plugin')// Thead插件
    module.exports = {
        ...
        module: {
    		rules: [
    			{
                    test: /\.js$/,
                    exclude: /node_modules/,
                    use: [
                        {
                          loader: 'thread-loader',
                          options:{
                              works: threads,// 指定工作进程数
                          }
                        },
                        'babel-loader',
                    ]
                },
    		],
    	},
        plugins: [
            new eslint({
                context: path.resolve(__dirname, 'js'),
                exclude: /node_modules/,
                threads, // 开启多线程并设置进程数
            }),
        ],
        optimization: {
            // 放置压缩的操作
            minimizer: [
                new cssMini(),// 压缩css
                new terser({ // 压缩js
                    parallel: threads,// 开启多线程和设置进程数量
                })
            ]
        }
    }
    ```

### Tree Shaking

- ==术语==，移除JS中没有用上的代码
- Webpack已经默认开启，无需配置
  - 注：依赖`ES Module`，否则无法应用

### Babel

- Babel为编译的每个文件都插入了辅助代码`_extend`，会增加代码体积，可以使用`@babel/plugin-transform-runtime`将辅助代码作为独立模块，避免重复引入

- `@babel/plugin-transform-runtime`

  - 禁用Babel自动对每个文件的runtime注入，而是引入，并使所有辅助代码从这里引入

- 使用

  - 安装：`npm i @babel/plugin-transform-runtime -D`

  - 在`babel.config.js`中配置

    ```js
    module.exports = {
       ...
       plugins: ['@babel/plugin-transform-runtime']
    }
    ```

### Image Minimizer

- 仅对本地静态图片进行压缩，如果是在线链接就不需要

- 使用

  - 安装：`npm i image-minimizer-webpack-plugin imagemin -D`
  - 模式
    - 无损压缩：`npm i imagemin-gifsicle imagemin-jpegtran imagemin-optipng imagemin-svgo -D`
    - 有损压缩：`npm i imagemin-gifsicle imagemin-mozjpeg imagemin-pngquant imagemin-svgo -D`

  - 在`webpack.config.js`中配置

    ```js
    // 引入插件
    const mini_img = require('image-minimizer-webpack-plugin')
    // 详见webpack官方文档配置案例 较长且固定写法 不在此展示

### Code Split(重要)

- 打包时会将所有JS打包到一个文件中，对于渲染单个页面有很多不需要加载的部分
- `Code Split`功能
  - 分割文件：将打包生成的文件进行分割，生成多个JS文件
  - 按需加载：需要哪个就加载哪个

#### 多入口方式

- `webpack.config.js`配置

  ```js
  const html_plugin = require('html-webpack-plugin')
  module.exports = {
      // 多入口要用对象形式
      entry: {
          app: './src/app.js',
          main: './main.js'
      },
      output: {
          path: path.resolve(__dirname, 'bundle'),
          // 因为入口有多个，会输出多个文件，如果 filename:main.js 
          // 会生成多个main.js，导致发生覆盖最终只有一个main.js
          filename: '[name].js',// webpack命名方式，以entry属性名命名所有[name]的文件
      },
      pligins: [
          new html_plugin({
              template: path.resolve(__dirname, '/src/index.html')
          })
      ],
      ...
  }
  ```

- 会遇到的问题

  - 如果有个公共模块被多个入口文件引用，那么打包时就会将==公共模块==生成多个，这时需要将==复用模块==单独打包，让输出文件调用

    ```js
    module.exports = {
        ...
        optimization: {
            // 代码分割配置
            splitChunks: {
                chunks: 'all',// 对所有模块进行分割
            }
        }
    }
    ```

  - 使用`import(path)`按需加载，动态导入，代码分割时会将动态导入的文件代码拆分成单独的模块，在需要时自动加载(页面加载时不会发起请求，触发事件时发请求获取文件)

    ```js
    // 使用import导入模块 替代 require('path')
    document.getElementById('btn').onclick = ()=>{
        // import返回值是Promise对象
        import('./modules1.js').then(res => {
            // 加载成功
            res.default.fn(2,1)
        }).catch(err => {
            // 加载失败
        })
    }
    ```

#### 单入口方式

- `webpack.config.js`配置

  ```js
  const html_plugin = require('html-webpack-plugin')
  module.exports = {
      entry: './main.js',
      output: {
          path: path.resolve(__dirname, 'bundle'),
          filename: 'js/main.js',
      },
      pligins: [
          new html_plugin({
              template: path.resolve(__dirname, '/src/index.html')
          })
      ],
      optimization: {
          splitChunks: {
              chunks: 'all',
          }
      }
      ...
  }

## Code Split

- 给==模块==命名

  - 在js文件调用处

    ```js
    document.getElementById('btn').onclick = ()=>{
        // 动态导入同时设置打包文件名
        // /* webpackChunkName: 'modules2' */是webpack特殊命名规则 固定写法
        import(/* webpackChunkName: 'modules2' */'./modules2.js').then(({fn2})=>{})
    }
    ```

  - 还需要在`webpack.config.js`文件中配置

    ```js
    module.exports = {
        output: {
            // 给打包输出的其他文件命名 如：代码分割的文件
            // module是为了区分模块文件
            chunkFilename: 'js/[name].module.js',
            ...
        },
        ...
    }
    ```

- 静态资源统一命名

  ```js
  // webpack.config.js
  module.exports = {
      output: {
          // 图片、字体等通过 type:asset 处理资源命名方式
          assetModuleFilename: 'media/[hash:10][ext][query]',
          ...
      },
      ...
  }
  ```

## 打包TS文件

- 使用

  - 安装：`npm i typescript ts-loader -D`

  - 在`webpack.config.js`中配置

    ```js
    module.exports = {
        // 入口文件指定ts
        entry: './ts/main.ts',
        module: {
            rules: [
                {
                    oneOf:[
                        {
                            test: /\.ts$/,
                            use: [{
                                loader: 'ts-loader',
                                options:{
                                    // 必须要指定tsconfig.json文件路径
                                    configFile: path.resolve(...)
                                },
                            }],
                            exclude: /node_modules/,
                        }
                    ]
                }
            ]
        },
        ...
    }