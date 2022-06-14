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

------

![img](https://upload-images.jianshu.io/upload_images/6322775-ce4a638569113a95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

图标样式

------

当多个图表写到一起，同时做自适应时

> window.addEventListener('resize', function() {  
>
> ​       myChart.resize();
>
> })

下面这个方法不建议用，因为只能触发一次，这一次触发中有多个图表的话，只执行第一个触发的；而`**addEventListener**`可以可以循环添加多个`**handler**`，为每个图表`resize()`一次

> window.onresize = function() {    
>
> ​       myChart.resize()  
>
>  }; 

![img](https://upload-images.jianshu.io/upload_images/6322775-d598015320faf1a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Tips：1、如果就是想用`onresize`，可以用`**setTimeout**`包裹`windo.onresize`事件，让其延迟触发

------

[不同颜色的柱子](https://kylebing.blog.csdn.net/article/details/109769527)

------

echarts默认只加载第一次传过来的数据，后面值再怎么变也不会动。所以用接口请求数据的时候，因为是异步执行，先传过去的是初始值(默认值)。

![img](https://upload-images.jianshu.io/upload_images/6322775-c7c13ce4d3ed5753.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

动态加载数据

echarts的所有数据**都在setOption**中设置，只用**定时**程序把myChart.setOption包裹起来，就能**动态加载**数据；圆括号中有花括号是因为把option={}隐去了

想要异步加载数据，则是用axios等接口请求工具，(以axios为例)用.then包裹myChart.setOption({ })；

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

Tips：

  1、因为echarts的·**data必须是可遍历的**·，所以不能·监听·数据，即像·vue·那样给数据添加`**defineProperty**`是不行的，因为这样会使数据·**不可枚举**·，解决办法：可以利用JS特性，·**对象.创建属性**·来创建·**非响应式**·属性，从而可以被echarts接收。

  2、由此可见，echarts的数据并非响应式，即不会因为监听到传入data的值变动，而自动更新数据，每次数据变化都需要·**setOption**·来重新设置数据

------

**轴线设置：**

  轴线默认值是type=value，会自动排列值，以递增的方式排列

  ·**axisLine：{symbol}**·设置轴线两端(可以分开设置)的样式，比如箭头等

​    ·**axisLine：{symbolOffset}**·设置周线两端样式的偏移量，因为默认图形是与网格线平齐的，不太符合一般“轴线”样式需求，而偏移可以让其超出轴线取值范围

​    ·**axisLine：{lineStyle：{shadow...}}**·设置轴线阴影。因为轴线两端偏移会使样式和轴线脱离，而shadow相关设置可以将用阴影·伪造·轴线，来将两者连接，分别需要·shadowBlur(模糊距离)··shadowColor(阴影颜色)··shadowOffsetX/Y(偏移)·

  ·**axisTick**·轴线上的刻度

  ·**axisLbel**·轴线上的文字

​     ·**axisLbel：{alignWithLabel}**·是否与刻度对齐

  ·**splitLine**·分割线配置

------

**Grid网格：**

  ·**grid**·和轴线中的·**网格(splitLine)**·注意区分，前者是轴线以内的所有内容(**grid默认不显示**)，后者是分割线  在grid里设置网格样式、大小，但是默认是不会铺满整个canvas，距离上部的距离可以通过Y轴最大值调整，左右距离可以通过left、right负值

  ·**grid：{show：true}**·时才能设置样式

------

![img](https://upload-images.jianshu.io/upload_images/6322775-732f6a51c1a3cf45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)