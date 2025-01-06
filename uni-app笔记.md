# 遇到的问题整理

## 有现成的微信云开发环境，如何复用？

1. 使用HbuilderX开发，**必须**运行到==微信开发者工具==，才能使用微信开发工具中管理的云环境，毕竟是不同平台
2. **必须**创建名为`cloudfunctions`的目录，将其复制到`unpackage/dist/dev/mp-weixin`路径下
   - 因为微信开发者工具下，云函数的存放目录固定名称为`cloudfunctions`，其他名称识别不了
   - 官方教程的`new CopyWebpackPlugin`要安装`copy-webpack-plugin`，是为了跳过手动复制到`unpackage`路径这步，**但是只能**在`vue.config.js`中使用，如果项目用的vite打包项目，则**不能用这个插件**
3. 此时，在==微信开发者工具==，就有`cloudfunctions`目录，右键该文件夹，关联云开发环境，新建云函数，编写完逻辑后，将云函数的`config.json`、`index.js`、`package.json`复制到HbuilderX下的`cloudfunctions`目录，进行备份，因为下次运行到小程序环境，依然会丢失`cloudfunctions`
   - 注意！在HbuilderX编写云函数需按微信开发者工具中格式去写，不能用`uni.cloud`
   - 只有在HbuilderX自己的云服务器平台配置关联的云服务才能用`uni.cloud`

## 如何使用vant一类第三方组件库

1. 首先选择支持小程序的库，如vant只有`vant-weapp`版本才能运行到小程序

2. 因为不同于在原始环境下运行，如`vant-weapp`官方教程是直接在页面或组件的JSON文件内配置，但是unaipp没有对应的JSON文件，只能在`pages.json`给页面配置，但是组件想用vant得用vue选项式才能编译到小程序JSON中

   - `pages.json`

   ```js
   {
       "pages": [
           {
               "path": "...",
               "style": {
                   "usingComponents": {
                       "vant-button": "/wxcomponents/vant/button/index"
                   }
               }
           }
       ]
   }
   ```

   - 组件使用vant需要使用vue选项式

   ```vue
   <script>
   export default {
       mpType: 'component', // 表明这是一个组件
       config: {
           usingComponents: {
               'vant-button': 'wxcomponents/vant/button/index'
           }
       }
   }
   </script>
   ```

3. 所以**必须**创建名为`wxcomponents`的目录，将`npm`安装的vant包拷贝到该目录下

   - `wxcomponents`是固定写法，原因不明，换个目录名称就不行

4. 然后在`pages.json`中配置，相当于组件注册到全局了

   - 注意！只有写在`globaStyle`下才能编译到小程序的`app.json`

   ```json
   {
       "globaStyle": {
           "usingComponents": {
               "vant-button": "/wxcomponents/vant/button/index"
           }
       }
   }
   ```

5. 在页面和组件中使用

   ```vue
   <template>
   	<van-button type="primary">xxx</van-button>
   </template>
   ```

6. 引入vant组件样式

   - 注意！`css`、`wxss`等原始格式，只能在`App.vue`中引入，`main.js`只能引入`less`等需要编译的格式
   - 小程序中没有`main.js`，`main.js`引入的`less`等样式，通过编译后会合并到小程序的`app.wxss`中
   - `App.vue`中引入的样式也会合并到小程序的`app.wxss`

   ```vue
   // App.vue
   <style lang="less">
   // 注意！样式文件是common/index.wxss
   @import '/wxcomponents/vant/common/index.wxss'
   </style>
   ```

## uniapp如何使用vite搭建项目

1. 要在根目录下手动创建`vite.config.js`，会自动识别用vite编译打包项目

   - 如果用默认的`vue.config.js`，使用的是webpack进行编译打包

2. 配置`vite.config.js`

   - 注意！不要使用`@vitejs/plugin-vue`

   ```js
   // vue项目下vite.config.js
   import { defineConfig } from 'vite';
   import vue from '@vitejs/plugin-vue';
   import path from 'path';
   
   export default defineConfig({
   	plugins: [vue()],
       // 其他配置
   });
   
   // uniapp项目下vite.config.js
   import { defineConfig } from 'vite';
   import path from 'path';
   // 注意 引入的是uni 而不是vue 且 不能！与vue()一起使用 会冲突
   import uni from '@dcloudio/vite-plugin-uni';
   
   export default defineConfig({
   	plugins: [uni()],
       // 其他配置
   });
   ```

## vue项目和uniapp项目的区别

- **vue项目**如果运行在**Mac**电脑上，需要在`xx.config.js`中添加监听本地文件才能==热更新==，而**uniapp项目**无论在Mac还是window系统都不需要

  ```js
  export default defineConfig({
      // 其他配置
  	server: {
          // 监听本地文件
          watch: {
              usePolling: true
          }
  	}
  });
  ```

- **web页面**的css样式`*`表示所有标签元素，而小程序没有这个，所以在**uniapp项目**中开发小程序要用全部标签代替`*`

  ```less
  // 不能用*号 而是写明小程序元素标签
  page,text,view,navigator,swiper,swiper-item,image{
      margin: 0px;
      padding: 0px;
      box-sizing: border-box;
  }
  ```