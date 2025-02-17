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

## uniapp中如何使用页面跳转间的事件通道

- 使用vue选项式风格

  - 起始页

  ```vue
  <script>
  export default {
    methods:{
      fn() {
        uni.navigateTo({
          url: 'test?id=1&name=uniapp',
          events: {
            收到跳转页发来的数据(data) {}
          }
        }).then(res => {
          res.eventChannel.emit('往跳转页发送', ...参数)
        })
      }
    }
  }
  </script>
  ```

  - 跳转页

  ```vue
  <script>
  export default {
    onLoad: function(option) {
      // 注意 选项式有this直接调用api即可
      const 通道 = this.getOpenerEventChannel();
      // 发往起始页的方式
      通道.emit('收到跳转页发来的数据', ...参数);
      // 监听事件 获取上一页面通过eventChannel传送到当前页面的数据
      通道.on('往跳转页发送', (data) => {})
    }
  }
  </script>
  ```

- 如果是vue组合式风格

  - 注意！`setup`不需要在`onLoad`或`onMounted`生命周期内建立通道

  ```vue
  <script setup>
  // 需要引入 获取当前组件实例的api
  import { getCurrentInstance } from 'vue';
  const 组件实例 = getCurrentInstance().proxy
  // 通过实例调取通道api
  const 通道 = 组件实例.getOpenerEventChannel();
  // 其他同上
  通道.emit('收到跳转页发来的数据', ...参数);
  通道.on('往跳转页发送', (data) => {})
  </script>
  ```

## watch无法监听到defineProps

- uniapp中使用解构赋值编译到小程序会有些问题

  ```js
  // 如果是在vite项目下 解构赋值的形式没问题
  // 因为watch时会改成 () => props.show
  const { show } = defineProps(['show'])
  watch(()=>show,()=>{})
  
  // 但是在uniapp中 必须 用 props 进行取值
  const props = defineProps(['show'])
  watch(()=>props.show,()=>{})
  ```

## 使用vant的组件Dialog无法跳转

- 官方示例

  - 注意：`Pages`中引入的是`"van-dialog": "wxcomponents/vant/dialog/index"`

  ```vue
  <template>
  	<!-- 要显示弹窗，必须添加组件标签 且id="van-dialog"不能少！ -->
  	<van-dialog id="van-dialog" />
  </template>
  <script setup>
  // 注意与Pages中引入的不同
  import Dialog from '@vant/weapp/dialog/dialog';
  Dialog.confirm({
    title: '标题',
    message: '弹窗内容',
  })
  // 但实际上 无法触发后面的then和catch
  // 在模拟器中显示是promise对象 但因为未知原因 有<canvas>存在时无法显示弹窗
  // 而在真机模拟中 Dialog.confirm()得到的是 空对象{} 而非promise对象 无法触发后面的then和catch
    .then(() => {
      // on confirm
    })
    .catch(() => {
      // on cancel
    });
  </script>
  ```

- 因此，只能在组件标签上绑定对应的关闭和确认方法才能正常使用

  ```vue
  <template>
  	<van-dialog @confirm="turnTo" id="van-dialog" />
  </template>
  <script setup>
  import Dialog from '@vant/weapp/dialog/dialog';
  Dialog.confirm({
    message: '弹窗内容',
  })
  function turnTo(){
    uni.navigateTo({
      url: ''
    })
  }
  </script>
  ```

## scoped时无法修改vant组件样式

- 因为`<style scoped>`会在每个元素标签上加一个`class="data-id"`，编译后CSS为`.a.data-id > .b.data-id`，因为是==交集选择器==，比起组件全局样式的`.b`优先级更低，因此无法覆盖组件原本样式

  - 使用`::v-deep`可将`.data-id`去掉

  ```vue
  <style lang="less" scoped>
    .a {
      // 组件样式前加v-deep后 编译得.a.data-id > .b 因此可以覆盖样式
      ::v-deep .b {
        // 定制样式
      }
    }
  </style>
  ```

## 提示无法找到id为van-notify的节点

- 一种情况是没有在`<template>`中加`<van-notify id="van-notify" />`标签元素

- 另一种情况是需要设置`context`配置项

  ```js
  // 如果是组合式vue setup
  import { getCurrentInstance } from 'vue';
  const 组件实例 = getCurrentInstance().proxy;
  Notify({ 
    type: 'warning', 
    message: '无法新增重名宠物', 
    context: 组件实例
  });
  
  // 如果是小程序中原生写法或者选项式vue
  Notify({ 
    type: 'warning', 
    message: '无法新增重名宠物', 
    context: this
  });
  ```

- 还有一种情况，当前组件是子组件，当前组件添加`<van-notify id="van-notify" />`标签元素并==没有用，要往父组件添加==

  ```vue
  // 父组件
  <template>
  	<子组件 :父组件实例="组件实例"/>
    <van-notify id="van-notify" />
  </template>
  <script setup>
  const 组件实例 = ref(getCurrentInstance().proxy);
  // 或者传入父组件 调用Notify
  </script>
  
  // 子组件
  <script setup>
  const props = defineProps(['父组件实例']);
  Notify({ type: 'warning', message: '', context: props.组件实例 });
  </script>
  ```

## vant日历组件回显日期问题

- vant日历没有传入值回显的功能，因此得在`:formatter`日期格式化函数中修改日期属性，达到回显的目的

  ```vue
  <template>
  	<van-calendar
  					@select="选择日期($event)"
            <!-- 利用formatter回调函数回显 必须绑定英文方法否则报错 -->
  					:formatter="formatter"
  					type="range"
  				/>
  </template>
  <script setup>
  // 初始化时 从全局获取日期
  let 日期 = [store.state.起始日期, store.state.结束日期];
  // select事件在点击任意日期 后 执行
  function 选择日期(e) {
  	let [start, end] = e.detail;
  	日期 = [start, end];
  	if (!start || !end) {
  		// 必须选择开始结束日期 否则禁用按钮
  		禁用按钮.value = true;
  	} else {
  		禁用按钮.value = false;
  	}
  	// 更新函数体 重新触发日期格式化
  	formatter = (day) => {
  		return formatterFn(day);
  	};
  }
  // 但是 formatter 在初始化 和 点击任意日期 前‼️ 都会触发
  // 因此用一个响应式变量保存函数 在点击日期后再次赋值函数体从而重新执行日期格式化
  let formatter = (day) => {
  	return formatterFn(day);
  };
  function formatterFn(day) {
  	const month = day.date.getMonth() + 1;
  	const date = day.date.getDate();
  	// 回显全局存的日期
  	let [startDate, endDate] = 日期;
  	let start;
  	if (startDate) {
  		start = startDate.getTime();
  	}
  	let end;
  	if (endDate) {
  		end = endDate.getTime();
  	}
  	let cur = day.date.getTime();
  	// 回显前先清空原状态
  	if (day.type == 'start' || day.type == 'end' || day.type == 'middle') {
  		day.type = '';
  		day.bottomInfo = '';
  	}
  	// 重新添加开始到结束之间的日期
  	if (start && cur == start) {
  		day.type = 'start';
  	} else if (end && cur == end) {
  		day.type = 'end';
  	} else if (start && end && cur > start && cur < end) {
  		day.type = 'middle';
  	}
  	// 添加开始结束日期文字
  	if (day.type === 'start') {
  		day.bottomInfo = '入住';
  	} else if (day.type === 'end') {
  		day.bottomInfo = '离店';
  	}
  	return day;
  }
  </script>
  ```