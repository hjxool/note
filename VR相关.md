# ThreeJS

## 安装

- 主版本：`npm i three`
- 小程序版本：`npm i threejs-miniprogram`

## 创建

1. 先准备一个容器`<div>`

   - 小程序版本必须用`<canvas type="webgl">`

2. 引入ThreeJS

   - `import * as THREE from 'three'`
   - 小程序版本
     - `import { createScopedThreejs } from 'threejs-miniprogram'`

3. 创建3D场景三要素

   - 常规版

   ```js
   import * as THREE from 'three';
   // 创建场景
   const scene = new THREE.Scene();
   // 观察视角的摄像头 PerspectiveCamera表示透视摄像头
   const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
   // 渲染器 动态渲染canvas的核心
   const renderer = new THREE.WebGLRenderer();
   // 设置渲染尺寸
   renderer.setSize( window.innerWidth, window.innerHeight );
   // 将 渲染器生成的 domElement 添加到页面容器下
   document.body.appendChild( renderer.domElement );
   ```

   - 小程序版本

   ```vue
   <template>
     <canvas id="threeScene" type="webgl"></canvas>
   </template>
   <script setup>
   import { createScopedThreejs } from 'threejs-miniprogram';
   let THREE;
   let canvas;
   // 小程序没有document 而是调用API创建query选择器 来获取容器节点
   const query = uni.createSelectorQuery();
   query
   	.select('#threeScene')
   	.node()
   	.exec((res) => {
     // 获取元素节点 但是不同于浏览器环境 所以不能直接用
   		canvas = res[0].node;
     // threejs常规版本需要 appendchild addEventListener 而这些小程序框架中都没有
     // 因此使用 createScopedThreejs 与canvas容器进行关联 得到THREE对象
   		THREE = createScopedThreejs(canvas);
     
     // 创建3D场景三要素
     const 场景 = new THREE.Scene();
     const 摄像头 = new THREE.PerspectiveCamera(75, canvas.width / canvas.height, 0.1, 1000);
     // 这里的使用场景是在模型内部查看 为尽可能在中心位置 设置为0.1
     摄像头.position.z = 0.1;
     // 配置项 antialias抗锯齿
     const 渲染器 = new THREE.WebGLRenderer({antialias: true})
   });
     // 设置 WebGL 渲染器的像素比 使其在高分辨率时 自动调整分辨率
     渲染器.setPixelRatio(uni.getSystemInfoSync().pixelRatio);
     // 注意！小程序版 没有 innerWidth 等属性
     渲染器.setSize(canvas.width, canvas.height);
    	// 伽马校正 使图像看起来更加真实和自然
     渲染器.gammaOutput = true;
     // 伽马校正因子 调整输出图像的亮度和对比度
   	渲染器.gammaFactor = 2.2;
   </script>
   ```

## 创建场景内的几何体

- 立方体

  - 常规版

  ```js
  // 旧版 使用BoxGeometry 优点是易于理解操作 缺点是性能较差
  // 参数为 宽 高 深
  const geometry = new THREE.BoxGeometry( 1, 1, 1 );
  // 新版 使用BoxBufferGeometry 优点传输速度快
  const geometry = new THREE.BoxBufferGeometry( 1, 1, 1 );
  // 设置贴图材质
  // 例 设置为基础颜色
  const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
  // 创建 网格 网格可以理解为图形的骨架
  const cube = new THREE.Mesh( geometry, material );
  // 往场景中添加网格图形
  scene.add( cube );
  // scene.add默认设置在0，0点位置 与摄像头默认位置重叠
  // 为了避免 移开摄像头
  camera.position.z = 5;
  ```

  - 小程序版
    - 以立方体贴图为例

  ```vue
  <script setup>
  // 立方体 6个面 的贴图
  const imgs = [];
  const 立方体对象 = new THREE.BoxBufferGeometry(10, 10, 10);
  // 设置材质贴图
  const 材质 = [];
  // 纹理加载器
  const loader = new THREE.TextureLoader();
  for (let url of imgs) {
    // 传入回调函数 在纹理加载完后 创建纹理对象 添加到材质列表
    loader.load(url, (texture) => {
      材质.push(new THREE.MeshBasicMaterial({map: texture}))
    });
  }
  const 网格 = new THREE.Mesh(立方体对象, 材质);
  // 注意！图片默认朝外 要在内部观察 所以要反转Z轴 -1表示翻转面
  网格.geometry.scale(1, 1, -1);
  场景.add(网格);
  </script>
  ```

## 渲染到页面

- 常规版

  ```js
  // 声明每帧绘制方法
  function animate() {
    // 浏览器自带API 相比于setInterval 好处是切换到其他页面时 绘制会暂停
    // 表示 每帧调用 animate 进行绘制
  	requestAnimationFrame( animate );
    // 当前帧 调用渲染器render方法 将场景内图形 及摄像头绘制到页面
  	renderer.render( scene, camera );
  }
  // 创建完场景和几何体立即调用
  animate();
  ```

- 小程序版

  ```js
  import { onBeforeUnmount } from 'vue';
  onBeforeUnmount(() => {
  	// 页面关闭时卸载渲染任务 否则会一直执行
  	canvas.cancelAnimationFrame(renderId);
  });
  
  function 绘制() {
    // 浏览器环境 自带requestAnimationFrame
    // three-platformize框架 在THREE对象下的$requestAnimationFrame
    // 小程序的requestAnimationFrame 则在 canvas标签元素 身上
    // 注意！小程序不会停止绘制 因此要存储 帧id 在页面unload时销毁
    renderId = canvas.requestAnimationFrame(绘制);
    // 渲染前要修改的操作都要加在render前
    // 网格.rotation.x += 0.01;
    // 网格.rotation.y += 0.01;
    渲染器.render(场景, 摄像头);
  }
  绘制();
  ```

## 控制摄像头移动

- 常规版

  ```js
  // 控制器必须从外部引入 threejs本身不包含
  import {OrbitControls} from 'three/example/OrbitControls'
  // 添加控制器
  const controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true; // 添加惯性
  controls.rotateSpeed = 6; // 调整旋转速度
  
  function animate() {
  	requestAnimationFrame( animate );
    // 如果开启了enableDamping(阻尼惯性)则必须在添加这句
    controls.update();
  	renderer.render( scene, camera );
  }
  animate();
  ```

- 小程序版

  ```vue
  <template>
  	<canvas id="threeScene" type="webgl" @touchstart="touchstart" @touchmove="touchmove" @touchend="touchend"></canvas>
  </template>
  <script setup>
  // 小程序版本的 控制器 是个 函数 因此不能解构取值
  import Controller from 'a/3D控制器.js';
  let THREE;
  let canvas;
  let renderId = null;
  let 控制器;
  const query = uni.createSelectorQuery();
  query
  	.select('#threeScene')
  	.node()
  	.exec((res) => {
  		canvas = res[0].node;
  		THREE = createScopedThreejs(canvas);
      init()
  	});
  function init() {
    // 其他操作
    
    // 添加控制器
  	// 注意！小程序版本控制器是个函数
    // 必须先传入THREE对象 再结构返回值得到控制器对象
  	const { OrbitControls } = Controller(THREE);
  	// pc端 new OrbitControls 传入摄像头和元素节点后就可以控制了
  	// 小程序版本 必须 配置touchstart等方法才能控制！
  	// 注意！小程序版本 传入canvas节点没有用 得传入 渲染器 生成的 domElement
  	控制器 = new OrbitControls(摄像头, 渲染器.domElement);
  	控制器.enableDamping = true;
  	控制器.rotateSpeed = 6;
    
    function 绘制() {
  		renderId = canvas.requestAnimationFrame(绘制);
  		控制器.update();
  		// 当前帧渲染
  		渲染器.render(场景, 摄像头);
  	}
  	绘制();
  }
  function touchstart(e) {
    // 必须 调用小程序画布的dispatchTouchEvent方法
    canvas.dispatchTouchEvent({ ...e, type: 'touchstart' });
  }
  function touchmove(e) {
  	canvas.dispatchTouchEvent({ ...e, type: 'touchmove' });
  }
  function touchend(e) {
  	canvas.dispatchTouchEvent({ ...e, type: 'touchend' });
  }
  
  </script>
  ```

## 父子元素

- 子元素相对于父元素==移动和缩放==

```js
const geometry = new THREE.BoxBufferGeometry( 1, 1, 1 );
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
// 父元素
let parent = new THREE.Mesh( geometry, material );
// 子元素
const child = new THREE.Mesh( geometry, material );
// 关联父子关系
parent.add(child)
// 将父元素添加到场景 子元素也会添加进去
scene.add(parent);
```

## 自适应画布大小

```js
window.addEventListener('resize', () => {
  // 重置渲染宽高
  renderer.setSize(window.innerWidth, window.innerHeight)
  // 重置相机宽高比
  camera.aspect = window.innerWidth / window.innerHeight
  // 更新相机投影矩阵
  camera.updateProjectionMatrix()
})
```

## 场景和相机关系

- 一个场景中可以存在多个相机，但是同时只能有一个相机的观察视角，也就是在==动画循环==中`renderer.render(scene, currentCamera)`切换`currentCamera`的相机实例
- 如果要创建多个视角，如VR看房中，还有平面图，其实也是一个场景，场景中包含一个平面图，以及一个正交相机`OrthographicCamera`
  - 如果有多个场景，那么==动画循环==中就要多次调用`renderer.render`，毕竟`renderer.render`参数只有一个`scene`

## canvas元素会盖住其他页面元素

- 这是因为旧版本小程序Canvas的`type="webgl"`特殊性所致，即使 `<canvas>` 写在前面，它依然会覆盖后面的元素
  - 小程序给出 `<cover-view>` 组件包裹非 Canvas 内容，用于覆盖在原生组件（如 Canvas）上方
- 另一种情况是模拟器显示错误，只能在真机调试看到实际效果