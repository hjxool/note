## 文件结构

- `public`：存放html文件以及ico图标
  - 将外部引入的文件库放在public里，因为这里有HTML文件，通过`<link>`引入的样式就不会被vue`严格审查`，从而避免添加一些不需要的附属文件库

- `src`：主目录
  - 存放组件文件和静态资源、图片，文件入口
  - 放`main文件`和组件管家`app组件`
- `assets`：放图片等静态资源

- `components`：子组件存放位置

- `.gitignore`：git上传时要忽略的文件

- `babel.config.js`：babel插件，打包时将语法转成兼容性更好的

- `package.json`：版本、使用的依赖库、常用命令等信息，使用npm就有这些东西

- `package-lock.json`：包管理文件，用于锁定依赖版本

## 样式

- `<style scoped>`：避免class名冲突，将样式划分成作用域，只作用于当前组件内的标签

## 组件间传参

- 最基础的方法
  - 父传子
    - 子组件用`props`接收，父组件传入的参数
  - 子传父
    - 父组件通过`props`传入一个函数，子组件中调用该方法，将值传入

- 全局事件总线

  - vuex

    - 安装
      1. `npm i vue@3`
      2. `src`文件夹下创建`store/index.js`
      3. `index.js`中创建==三个==对象(可以自定义名)`actions`、`mutations`、`state`，分别用于发起commit请求、实际操作、存储公共数据
      4. `index.js`中`import vue和vuex`，然后`Vue.use(Vuex)`
      5. `index.js`中`export default new Vuex.Store({actions, mutations, state})`

    - 使用
      - 需要通过commit进行判断、请求时

## 解决跨域问题

- 通过代理服务器

![img](https://upload-images.jianshu.io/upload_images/6322775-4bc19d088d9b4e1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center>在vue.config.js中配置</center>

- TIps

  - 可以在`/api`这块位置直接写服务器**根路径**，就不用重写地址了

  - `proxy`中可以配置多个服务器，以对象的形式配置参数

  - `target`只写**端口号**和**IP地址**
  - 在发送请求时，写`本地IP:本地端口/xxx/服务器路径`

## 切换显示vue router

- 安装

  1. `npm i vue-router@3`

  2. 在`src`下创建`router/index.js`，导入`VueRouter`，以及想要切换显示的**组件**

  3. ```js
     export default new VueRouter({
         path: '/xxx',
         component: {},
     })
     ```

  4. 在`main.js`导入`VueRouter`、`router/index.js`

  5. 

## 注意事项

- vue脚手架默认是不带==TS==模块的，想要使用ts，除了`<script lang="ts">`还需要在命令行`vue add typescript`或`npx vue add typescript`，即可往当前项目下添加TS模块
- `npm run build`打包生成的文件需要在==服务器==上才能运行