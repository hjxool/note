## 标签属性

- `<svg>`：顶层标签

  - `width`和`height`：直接设置宽高，支持百分比和`px`
  - 也可以用css设置样式
  - `viewBox="x y width height"`：设置可见区域位置和范围
    - 前两个参数是可见区域==起始==位置坐标
    - 后两个参数是可见区域大小
    - 注：切出来的区域会==放大==到`<svg>`整体大小
  - `<svg>`表示一层画布，==画布中的图形==没有`z-index`这样的层级关系，并且`<svg>`没有背景色，是透明的，因此多个`<svg>`可以叠加显示，但是`<svg>`有`z-index`层级，所以在上面的画布，它的图形会显示在上层

- `<circle>`：画圆

  - `cx`和`cy`：==圆心==的x，y轴坐标
  - `r`：圆半径
  - 可以通过css设置的标签属性
    - `fill`：填充颜色
    - `stroke`：边框颜色
    - `stroke-width="3px"`：边框宽度

  ```html
  <svg width="100%" height="100%">
  	<circle cx="100" cy="100" r="50" fill="red"></circle>
  </svg>
  ```

- `<line>`：直线

  - `x1 y1`：第一个点的坐标
    - 两点才能成线
  - `x2 y2`：第二个点的坐标
  - `<line>`==只能有2个点==
  - 同样用`stroke stroke-width`属性可以设置线段颜色和粗细

  ```html
  <line x1="50" y1="50" x2="200" y2="200"></line>
  ```

- `<polyline>`：折线

  - `points`：折线的==端点==坐标
    - 横坐标和纵坐标用逗号隔开
    - 端点间用空格隔开
  - 折线默认有填充颜色。使用`fill: none;`清除默认样式
  - `stroke stroke-width`：设置线段颜色粗细

  ```html
  <polyline points="50,50 100,100 120,100 140,80"></polyline>
  ```

- `<rect>`：矩形

  - 只需要起始点以及宽高就可以绘制
  - `x y`：矩形左上角端点坐标
  - `width height`：矩形宽高

  ```html
  <rect x="0" y="0" height="100" width="200"></rect>
  ```

- `<ellipse>`：椭圆

  - `cx cy`：同圆
  - `ry`：纵轴半径
  - `rx`：横轴半径

  ```html
  <ellipse cx="50" cy="50" ry="40" rx="20"></ellipse>
  ```

- `<polygon>`：多边形

  - `points`：同折线

  ```html
  <polygon points="50,50 100,100 120,100 140,80"></polygon>
  ```

- `<path>`：绘制路径

  - `M`(moveTo)：起始点

  - `L`(lineTo)：画一条直线到某点

  - `H`：画一条水平线到某点

    - 只需要给x轴坐标

  - `V`：画一条垂直线到某点

    - 只需要给y轴坐标

  - `Q`：二次贝塞尔曲线

    - `Q 100 70(控制点), 190 10(终点)`

  - `T`：平滑二次贝塞尔曲线

    - 只需要设置一个终点，相比`Q`，`T`不需要设置控制点，它的控制点是通过==前一个Q**或**T命令==自动计算出来的

      ![image-20231008172938539](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20231008172938539.png)

      ```html
      <svg>
          <path d="
                  M 10 75
                  Q 55 10 100 75
                  T 190 75
          "></path>
      </svg>
    
  - `C`：三次贝塞尔曲线

    - 三次贝塞尔和二次的区别是曲线弯度，不是有几个拐点！想多几个拐点的曲线只需要路径中增加`Q`或`T`的点位即可

    - 三次贝塞尔曲线有2个控制点，语法`C x1 y1(控制点1) x2 y2(控制点2) x y(终点)`

      ![image-20231009102512553](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20231009102512553.png)

      ```html
      <svg>
          <path d="
              M 10 10
              C 20 130, 160 130, 170 20
          "></path>
      </svg>

  - `S`：平滑三次贝塞尔曲线

    - 类似`T`是`Q`的简写，`S`是`C`的简写，只需要设置一个控制点。语法`S 160 130, 170 20`

  - `A`：弧形

  - `Z`(可选)：从结束点到开始点画一条直线，形成一个闭合的区域

  - 以上所有命令均允许小写字母

    - 大写表示绝对定位，以==svg左上角==为参照
    - 小写表示相对定位，以==上一个点==为参照

  ```html
  <path d="
           M 18,3 也可以 18 3 表示横纵坐标
           L 46,3
           L 46,40
           L 61,40
           L 32,68
           L 3,40
           L 18,40
           Z
           "></path>
  ```

- `text`：绘制文本

  - `x y`：文本==基线==起点坐标
  - 文字样式可以用CSS设置
  - 主要用于设置镂空字体等
    - 使用`stroke`描边，`fill`填充空白，不需要设置`color`


  ```html
  <text x="50" y="25"></text>
  ```

- `<use>`：复制图形

  - `href`：复制目标图形标签身上的css选择器
  - `x y`：复制到什么坐标位置

  ```html
  <circle id="circle" cx="100" cy="100" r="50" fill="red"></circle>
  <use href="#circle" x="100" y="500"></use>
  ```

- `<g>`：将多个图形分到一组

  - 配合`<use>`整体复制
  - 注！不能设置宽高！大小随内容撑大

  ```html
  <svg>
  	<g id="group1">
      	<circle cx="100" cy="100" r="50" fill="red"></circle>
          <circle cx="100" cy="100" r="50" fill="red"></circle>
          <circle cx="100" cy="100" r="50" fill="red"></circle>
      </g>
      <use href="#group1" x="100" y="200"></use>
  </svg>
  ```

- `<defs>`：内部的图形只作声明引用，不显示

  - 配合`<group>`使用

  ```html
  <svg>
  	<defs>
          <!--声明group1 但不显示 只用作其他地方引用-->
      	<g id="group1">
              <circle cx="100" cy="100" r="50" fill="red"></circle>
              <circle cx="100" cy="100" r="50" fill="red"></circle>
              <circle cx="100" cy="100" r="50" fill="red"></circle>
      	</g>
      </defs>
      <use href="#group1" x="100" y="200"></use>
  </svg>
  ```

- `<pattern>`：使用自定义图形铺满指定区域

  - `<pattern>`需要设置大小，这样可以用固定大小填满对应容器
  - `patternUnits="userSpaceOnUse"`：表示`<pattern>`的宽高是实际像素值
  - `<rect>`的`fill="url(#dots)"`表示使用对应id图形来填充

  ```html
  <defs>
      <!--一组图形区域大小-->
  	<pattern id="dots" x="0" y="0" width="100" height="100" patternUnits="userSpaceOnUse">
      	<circle fill="red" cx="50" cy="50" r="20"></circle>
      </pattern>
  </defs>
  <!--绘制一个矩形 内部用对应id的pattern填满-->
  <rect x="0" y="0" width="100%" height="100%" fill="url(#dots)"></rect>
  ```

- `<image>`：插入图片

  - `xlink:href`：图像来源

  ```html
  <image xlink:href="xxx.png" width="50%" height="50%"></image>
  ```

- `<marker>`：标记

  - 常用于给线段添加箭头，写在`<defs>`中用于可复用图形

    - 线段设置css样式或者标签属性`marker-start/mid/end: url(#marker_id)`即可添加箭头
      - `marker-mid`不同，它是在==线的转折点==添加，因此像`M 10 10 L 100 100`这种没有中间点的线，`marker-mid`属性无效
    - `overflow="visible"`一定要加，因为在线段首尾的箭头超出线段容器可视区域，会隐藏
  
  - `markerWidth markerHeight` 设置标识大小
  
  - `markerUnits="userSpaceOnUse"`一定要加，不然画的是`10x10`px大小的图标，会自动放大
  
  - `refX refY`：表示吸附在线段==端点==的==基准点==
  
    - ==图标原点==：`<marker>`内绘制图形的==起始点==为`0,0`点，也是==默认基准点==
  
      - 如`<path d="M 0 0 L 8 -4 L 6 0 L 8 4 Z">`绘制的图标为
  
        ![image-20231011161723332](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20231011161723332.png)
  
        - 原点在箭头处，以原点为基准的坐标轴
  
          ![image-20231011162153843](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20231011162153843.png)
  
        - 使用`marker-end:url(#arrow)`配合`<marker id="arrow" orient="auto">`得到的图形结果
  
          ![image-20231011162723903](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20231011162723903.png)
  
        - 再`transform:rotate(180deg)`将图标旋转180°，既可得到，可以看到线段终点和箭尖重合
  
          ![image-20231011163106782](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20231011163106782.png)
  
        - `transform`旋转坐标轴不会跟着转，但是`<marker orient="45">`会使坐标轴跟着转
  
          ![image-20231011163529766](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20231011163529766.png)
  
        - 将旋转后的图标沿X坐标轴负向移动`<marker refX="-10">`10px，得到
  
          ![image-20231011164320093](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20231011164320093.png)
  
  - `orient`：绘制的方向(角度值或`auto`)
  
    - `auto`表示以随线段方向自动变化
      - 如==线段==起始点`0,0`，终止点`100,100`斜率45，那么`<marker>`图标就要==以图标**原点**==顺时针转45°


  ```html
  <defs>
  	<marker refX="2" refY="6" orient="auto" markerUnits="userSpaceOnUse"
              overflow="visible"></marker>
  </defs>
  ```

- `<foreignObject>`：在svg中插入HTML标签

  - 默认以`<svg>`左上角为原点，多个`<foreignObject>`会重叠在一起，跟`<g>`一样只能用`transform`样式属性挪动

  ```html
  <foreignObject style="width:100px;height:100px">
  	<div>{{可以使用Vue相关语法}}</div>
  </foreignObject>

## 使用stroke虚线实现伸长变幻动画

- 常见的加载圆圈就是用`stroke-dasharray`以及`stroke-dashoffset`结合`animation`实现的

- `stroke-linecap`：线头样式

  - `stroke-linecap: 'round'`：线头是弧形

  - `stroke-linecap: 'butt'`：默认样式

  - `stroke-linecap: 'square'`：方形

- `stroke-dasharray`：创建虚线

  - `stroke-dasharray="100"`一个参数表示，虚线长`100`间隔(空白)`100`

    ![image-20230920141811364](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20230920141811364.png)

  - `stroke-dasharray="100, 20"`两个参数表示，虚线长`100`间隔(空白)`20`

    ![image-20230920142019143](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20230920142019143.png)

  - `stroke-dasharray="100, 20, 30"`多个参数表示，虚线长`100`间隔`20`虚线长`30`，**接着**间隔`100`，虚线长`20`，间隔`30`如此循环

- `stroke-dashoffset`：==相对起始点==偏移

  - 注：不论向哪个方向偏移，`stroke-dasharray`的虚线都是循环的

  - 值为**正**时

    - 对于直线是==从终点向起点==偏移

    - 对于圆是==逆时针==偏移

      - 圆的起始点在==右边中间==

      - 偏移单位是`px`，即转过的弧形长度
      
      - 注！圆形的**终点**和==默认起始点==是重合的，超过终点的`stroke-dasharray`不会显示！
      
        - 如：`stroke-dashoffset: 60`，是把起始点逆时针转`60px`长度，这时即使`stroke-dasharray: '10000,1'`，虚线长度也只会显示`60px`，因为`dashoffset`只能修改起始点的偏移，终点还在原来起始点位置上，相当于虚线显示区域只有`60px`长度
        
        ![image-20230920142955729](C:/Users/admin/AppData/Roaming/Typora/typora-user-images/image-20230920142955729.png)

  - 值为**负**时

    - 对于直线是==从起点向终点==偏移
    - 对于圆是==顺时针==偏移

- 加载转圈实例

  - HTML页面

  ```html
  <svg width="100" height="100">
  	<circle class="loading" r="20" cx="50" cy="50"></circle>
  </svg>
  ```

  - CSS样式

  ```css
  .loading{
      stroke-width: 3px;
      stroke: #1989fa;
      /*设置动画 1.5s一轮 无限循环*/
      animation: circle 1.5s ease-in-out infinite;
  }
  @keyframes circle {
      0% {
          /*虚线长1px 空白长度大于圆的周长2Πr 120左右就行*/
          stroke-dasharray: '1, 150';
          /*开始无偏移*/
          stroke-dashoffset: '0';
      }
      50% {
          /*虚线长90px 间隔长度大于周长 显示效果就是只有90px长度可见 其余空白*/
          stroke-dasharray: '90, 150';
          /*起始点顺时针偏移40px距离*/
          stroke-dashoffset: '-40';
      }
      100% {
          stroke-dasharray: '90, 150';
          /*虚线长度不变 但是起始点顺时针转120px*/
          stroke-dashoffset: '-120';
      }
  }

## Tips

- 与`canvas`不同的是，`svg`内部都是标签，更便于css样式动画等以及DOM节点交互