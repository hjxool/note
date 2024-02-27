[挑选Echarts模板](https://echarts.apache.org/examples/zh/index.html#chart-type-pie)

[echarts饼图样式](https://www.jianshu.com/p/3c7523cc7a6b)


## 提示框：tooltip

- 额外的小功能，包括配置鼠标滑过或者点击后的显示框；

- trigger：触发方式；

- formatter：以何种方式显示内容

  - 简单模板：

  ![简单模板](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220614175753017.png)

  - 自定义模板：

    ```js
    formatter:(params)=>{
        params里有表格所有内容，取出重新排布以后
        return 字符串内容
    }
    ```

- legend：图例显示方式；包括位置、大小、内容等；只要series中写了data[ ]，legend中的data就可以省略

- axispointer：坐标轴指示器，默认为line直线，cross十字准星，shadow阴影

## 图例

- ![img](https://upload-images.jianshu.io/upload_images/6322775-ce4a638569113a95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ==orient==：图例横纵排列方式。有`'horizontal'`和`'vertical'`
- ==textStyle==：图例文字(统一)样式
  - ==color==：文字颜色

## 自适应

- 当==多个图表==写到一起，同时做自适应时

> window.addEventListener('resize', function() {  
>
> ​       myChart.resize();
>
> })

- ==onresize==不建议用，因为只能触发一次，这一次触发中有多个图表的话，只执行第一个触发的；而`**addEventListener**`可以可以循环添加多个`**handler**`，为每个图表`resize()`一次

> window.onresize = function() {    
>
> ​       myChart.resize()  
>
>  }; 

- [resize]()：方法。所有参数都可以缺省，可传入的参数有(从前往后)：
  - ==width==指定宽度，可传==null/undefined/'auto'==表示自动取容器高度、==height==指定高度，传值同上、==silent==是否禁止抛出事件，默认为false、==animation==宽高改变时是否应用==过渡动画==，包含==duration时长==/==easing缓动==两个配置项，默认==不应用过渡动画==
- Tips：
  - 如果就是想用`onresize`，可以用`**setTimeout**`包裹`windo.onresize`事件，让其延迟触发

## 更新数据

- echarts默认只加载第一次传过来的数据，后面值再怎么变也不会动。所以用接口请求数据的时候，因为是异步执行，先传过去的是初始值(默认值)。

![img](https://upload-images.jianshu.io/upload_images/6322775-c7c13ce4d3ed5753.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- echarts的所有数据**都在setOption**中设置，只用**定时**程序把myChart.setOption包裹起来，就能**动态加载**数据；圆括号中有花括号是因为把option={}隐去了

- 想要异步加载数据，则是用axios等接口请求工具，(以axios为例)用.then包裹myChart.setOption({ })；

> axios({  
>
> ​    method: 'post',
>
> ​    url: '564564',
>
> ​    headers: {'token': params}
>
> })
>
>   .then(function(res) {
>
> ​      myChart.setOption({ data = res.data});
>
> });

- Tips：
  - 因为echarts的**data必须是可遍历的**，所以不能监听数据，即像vue那样给数据添加`defineProperty`是不行的，因为这样会使数据**不可枚举**，解决办法：可以利用JS特性，**对象.创建属性**来创建**非响应式**属性，从而可以被echarts接收。
  
  - 由此可见，echarts的数据并非响应式，即不会因为监听到传入data的值变动，而自动更新数据，每次数据变化都需要**setOption**来重新设置数据
  
  - 再次使用`setOption(option)`更新数据时，不需要重新传入之前配置好的项，只需要将需要更新的字段传入
  
    ```js
    this.echartDom.setOption({
        series: [
            {data: newData}
        ]
    })

## 轴线设置

- 轴线默认值是type=value，会自动排列值，以递增的方式排列
- **axisLine：{symbol}**设置轴线两端(可以分开设置)的样式，比如箭头等

- **axisLine：{symbolOffset}**设置周线两端样式的偏移量，因为默认图形是与网格线平齐的，不太符合一般“轴线”样式需求，而偏移可以让其超出轴线取值范围

- **axisLine：{lineStyle：{shadow...}}**设置轴线阴影。因为轴线两端偏移会使样式和轴线脱离，而shadow相关设置可以将用阴影·伪造·轴线，来将两者连接，分别需要·shadowBlur(模糊距离)··shadowColor(阴影颜色)··shadowOffsetX/Y(偏移)·

- **axisTick**轴线上的刻度

- **axisLbel**轴线上的文字

- **axisLbel：{alignWithLabel}**是否与刻度对齐

- **splitLine**分割线配置

## 网格Grid

- **grid**和轴线中的**网格(splitLine)**注意区分，前者是轴线以内的所有内容(**grid默认不显示**)，后者是分割线  在grid里设置网格样式、大小，但是默认是不会铺满整个canvas，距离上部的距离可以通过Y轴最大值调整，左右距离可以通过left、right负值

- **grid：{show：true}**时才能设置样式

- 用于==拉伸展示区域==

- 只作用于四周的外边框

## 颜色样式

- ![img](https://upload-images.jianshu.io/upload_images/6322775-732f6a51c1a3cf45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- [不同颜色的柱子](https://kylebing.blog.csdn.net/article/details/109769527)

## 饼图

- ==修改图表位置==
  - 在设置数据的[series]()里
  - ==center:["50%","50%"]==：圆心坐标
  - ==radius["20%","50%"]==：饼图半径。第一个参数是==内径==，第二个参数是==外径==。参数可以是number类型，表示直接指定值；也可以string类型，表示==相对可视区域的百分比==
- ==label==：饼图上的文字显示
  - ==show==：是否显示==默认==标签。默认参数为==true==。
  - ==position==：显示位置。`'outside'(扇形外围，有引导线) 'inside'(饼图区域)  'center'(圆心处) `。同时会影响高亮区域的显示文字
  - ==formatter==：格式化内容。
    - ==字符串模板==：`"{a}"`系列==名==、`"{b}"`数据==名==、`"{c}"`数据==值==、`"{d}"`百分比。`formatter: '{b}: {d}'`
    - [回调函数]()：`formatter:(形参)=>{ 逻辑 return "字符串" }`
- ==emphasis==：高亮扇区的样式及文字样式
  - ==label==：没有==position==配置，受外层label的position影响
    - formatter作用等同外层label配置项。**但是！**同时设置时，==优先级==比外部label的formatter==高==
- ==labelLine==：==视觉引导线==的设置
  - ==lineStyle==：线的样式。
    - ==color==：颜色

## 漏斗图

- ==sort==：数据排序。可以传==ascending上升==、==descending降序==、==none按照data顺序==、==function(a,b)函数==
- ==itemStyle==：图形样式
  - ==borderWidth==：描边。默认为1，0是无描边

## 折线图

- ==areaStyle==：区域面积样式。空对象即可显示默认区域样式

## 共同配置

- ==label==和==emphasis==里的label都会影响图形标签显示、样式，且==emphasis==优先级更高
  - 想要让label消失，必须同时设置label和emphasis > label的show均为false，不然默认会显示

- ==color==：

  ```js
  线性渐变
  color:{
  	type: 'linear',
  	x: 0,	横向从右到左 0~1之间的值 表示百分比
  	y: 0,	纵向从下到上
  	x2: 0,	从左到右
  	y2: 1,	从上到下
  	colorStops: [{
  		offset: 0, 	0~1之间的值 表示从百分之多少开始渐变
          color: 'rgba(30,144,255,1)' // 0% 处的颜色
  	}, {
  		offset: 1, color: 'rgba(30,144,255,.4)' // 100% 处的颜色
  	}],
  	global: false // 缺省为 false
  }
  ```
  
  ## 一些注意事项
  
  - echart其实绘制在页面上是==一次性==的，即使实例被==销毁==，绘制上去的图表也==不会消失==。所以可以用==局部变量==