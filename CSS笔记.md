## Tips：

1. 即使对于绝对定位的元素，只要它横向或者纵向没设置定位，就可以用flex或者grid改变它横向/纵向的位置

2. grid和flex居中都是以盒模型中的content为准，所以padding会影响居中效果
3. 父元素未设宽高，子元素百分比无用
4. 使用flex并限制宽度可以使文字垂直显示
5. 用百分比设置宽高，会相对于父容器改变，但用vw/vh，在设置了最小宽高，且小于最小值的情况下，会出现与父容器脱节的情况，但是用vw和vh来限制宽高就不会了
6. 不应该给“<span>”设宽高，这样元素大小就会随着字体大小改变，而不会出现文字下划线不和谐
7. img和grid、flex相性不好，要用“div”将“img”包着才行
8. flex和grid没设置“align/justify”对齐方式时，子元素可以不用设置“width/height”，因为会默认填充满整个父容器
9. calc(xxx +/- xxx)中间必须有空格
10. “**grid-auto-flow**”为“column”时，列最后考虑，先从上往下填满行。为“row(默认值)”时，行最后考虑，先从左往右填满列
11. 滚动显示时想要平滑跳转可以使用“scrll-behavior：smooth(平滑)”属性
12. **grid、flex：**有宽高才能设置对齐属性
13. 自动填充滚动显示的容器用·**overflow：auto**·，与自动伸缩容器物理意义上(**不看层级**)**直接接触**的**层级最近父容器**要设置·**overflow：hidden**·，才能让自动填充的容器滚动显示。比如：往下嵌套三层的子容器想要滚动显示，而往上两层父容器都是自动伸缩的，那么就要看自动伸缩的最外层容器邻接的父容器有没有设置·**hidden**·(其实归结一句话，就是想要滚动显示，必须要让设置滚动的容器知道自己是被限制的！)
14. `border-radius`：跟宽高有关，当其值为最大边的一半，不论是`px`还是`50%`，就会是圆
15. ※**滚动显示**时，**主要**是用**外层父容器**来**限制**，从而使内部滚动容器**受限**从而开始滚动
16. ·**display：none**·的元素不会占用·grid/flex·布局位置
17. **·padding·**不影响**滚动条**，所以可以通过padding来**腾出**滚动条**空间**
18. 经验1：容器用布局样式，不要使用对齐方式，只设·**宽度**·，·**子元素**·用·**margin**·、·**padding**·设置·元素大小·，将容器撑起。另外·**子元素内**·始终都是要用布局对齐的。(但有一种特例，当子元素内有东西是忽隐忽现的，子元素大小会变化，所以还是得设置高度，因为父容器没有高度，是子元素撑起来的)
19. 元素排列方向既是·**主轴**·，而·**交叉轴**·默认都是·**拉伸**·的。在不用布局时，主轴是垂直往下走的，所以只设置高度都可以横向铺满。用·**flex**·时，主轴是横向的，所以只设置宽度，就可以纵向铺满。只有·**grid**·相较特殊，里面的元素横纵都是满铺
20. 遮罩和弹窗类元素写到一起，用==遮罩包裹元素==，遮罩设置`position:absolute;top/left/bottom/right:0;`居中显示里面的元素

------

[RGB颜色转换](https://www.sioe.cn/yingyong/yanse-rgb-16/)

------

常用class=“类名”来标记，多类名中间要用空格隔开；

.class{

   属性：值

}

------

id标记好比身份证号，一般用于页面唯一性的元素上，搭配js使用

\#id{

   属性：值

}

------

在<head>里写上<link rel="stylesheet" href="路径">引入样式表

------

CSS样式优先级：内联样式 > 外部样式，同样是外部样式写在后面的外部样式会覆盖前面的。

声明优先级：!important > 内联 > ID > Class|属性|伪类 > 元素选择器。计算方式是根据左大右小以及累加。

------

display的几种属性值：**inline， block，inline-block**

inline（行内元素）:不能更改height，width值，大小只能由内容决定；不会独占一行；可以使用padding全部属性以及margin左右属性。

block（块元素）：可以更改height，width值以及padding、margin全部属性；但是会独占一行；默认填满父元素宽度。

inline-block（行内块元素）：不独占一行的块元素。

**inline-block** 和 **float(浮动布局)**的区别：

inline-block：整齐，有间隙(可以为父元素添加**{font-size:0}来消除空白符大小**)

float：参差不齐(需要**子元素高度一致**才会显得整齐)，无间隙。浮动元素依然遵循盒模型。

总结：对于横向排列使用**inline-block**可以去除浮动，避免布局混乱；float用于**文字环绕**效果。

float：使目标元素靠左右放置，父元素设置后不会应用于子元素。子元素设置后，只要没有占满整行，会依次水平排列。父元素应用float布局后，宽度会依照子元素的最大宽度。

**清除浮动**：之所以会有浮动脱出标准流，是因为应用float的元素**会脱离正常布局**，而位于浮动元素(应用了float的元素)之下的**正常布局内容**会**围绕**浮动元素**，填满右侧区域，**所以要把**clear**添加到**浮动元素之下的正常布局元素**，来清除其两侧的浮动。

------

![img](https://upload-images.jianshu.io/upload_images/6322775-c743b6a11f347c74.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上标 下标

## flex和grid

**flex:**主轴默认**横向**排列，**flex-direction**决定项目排列方向(**想要让大大小小每一个元素自动占一行就用这个属性**)。

**flex-flow**属性是flex-direction(方向)属性和flex-wrap(换行)属性的简写形式。

flex是一种布局方案，应用flex的元素标签自动成为“容器”，容器下有6种属性，分别为方向、换行、方向+换行、横向(主轴)对齐方式、垂直方向(纵轴)对齐方式

stretch代表容器占据整个页面(上下左右)

注意**align-content**属性是**多轴**条件，所以下属的space-around和stretch参数都是针对轴线；stretch占满整个交叉轴，指纵向拉伸项目；space-around指轴线间的间隔而非项目间隔

注意，flex所属的元素将成为布局容器，旗下的元素比如<a>标签之类的有另外6种属性设置。分别为：顺序、放大倍数(都是1则平分空间，其中一个是2或者更多则分配的空间是1的2倍或者更多倍，而不是按照自身大小的倍数，注：默认值为0不放大)、缩小比例(注：默认值为1，表示空间足够的情况下不缩小，如果为0则什么情况下都不缩小，而1的项目平分剩下的空间)、基础大小、以及放大，缩小，基础大小的综合属性flex、单项目的对齐方式(可覆盖父容器的align-items，默认值是auto，表示继承父元素的align-items属性)

flex只能应用在子元素，不能用到子元素的子元素

flex布局下子元素**不能设置成absolute**,absolute会**脱离**布局控制，**relative**也会**无视**absolute的元素；**非flex布局**下即使用了absolute页面也会自动编排元素，但是设置了定位后就会脱离页面自动编排的位置，所以在设置**left和right**的时候元素不会叠到一起(**上下排布**的元素)，但是用**上下**定位的时候**会堆叠**

用flex排布字体会将div元素里的字当作最小的元素块移动位置

Tips：

1. 使用“**flex-grow**/flex-shrink”可以控制元素自动填充，是flex的自动填充方法，“flex-grow”默认不填充扩大，所以要单独设置
2. [flex-grow]()会使[width]()失效

**grid布局**：

  Tips：

​    1、想要自动填充滚动，用grid-auto-row/column将自动填充的内容设置为自己想要的大小

​    2、`**overflow：auto**`会在gird设置了`**对齐方式(比如align-items、justify-items)**`时**失效！**，所以往常能自动滚动是因为，里面没有设置对齐格式，让子元素自动填充网格再配合overflow才实现

​    3、自动撑大父容器不能使用百分比

​    4、**绝对定位不**会**占用**grid划分好的格子

​    5、父容器设置了宽高后，想要一行一行自动填充内容，可以用“grid-**template**-columns:repeat(**auto-fill**, **绝对值**);”，会根据容器大小自动填充

​    6、auto-fill是容器宽高设置的情况下尽可能往里填充元素，对那种宽高自动撑大的容器可以不用设置template-column/row；

​      tips：repeat(auto-fill)在固定大小的父容器中非常好用，会自动换行

​    7、**justify/align-content**是指分割块**铺不满**容器时，分割块的定位

​    8、如果想横向扩充，需要设置grid-auto-flow：column（先列后行），先填充满列再补充行，其实就是变相告诉浏览器是横向排列还是纵向；

​    9、·**template-col/row妙用！**·用于指定**起始**·列/行数·，比如·auto-flow：row(默认)·时，纵向滚动，默认是一行**只有一列**，而使用·**template-column:repeat(auto-fill/指定列数，绝对大小)**·，可以指定一行有多少列，然后向下滚动

​    10、 fr单位是将以知宽度按比例切割，不能用于撑大父元素

​    11、使用grid制作表格时，当表头和单元格尺寸不一时，grid-template能帮上忙，它能指定手写的行列为指定宽高

​    12、`**grid-gap**`：第一个参数是行间隔，第二个参数是列间隔

​    13、设了宽高的情况下使用百分比或者绝对大小自动填充看起来是一样的，但是滚动的时候超出原始宽高的部分还是会自动压缩

​    14、使用·**template-row/column**·时，元素会自动填充，而不会自动垂直排列。只有超出·**template(模板)**·的才会，所以才需要·**auto-flow:column**·来规定**多出来**的元素如何排布

​    15、**默认**会**自动拉伸**，但是当设置align或justify对齐时，会自动将未设宽高的元素压缩至最小；这与·**flex**·不同，flex只会自动拉伸高度，在·direction：column·时，则会拉伸·**宽度**·，因为主轴变为纵轴时，原来的高度就变为了宽度

------

line-height是行高，在一行中字体是默认居中的，当行高与块级元素高度相同，字体就垂直居中了。

**页面居中**：绝对或相对定位、top等属性设为0、margin：auto；左右居中就left，right为0，margin：auto（==但是会拉伸未设宽高的地方==）

​         left：50%（移动边缘），margin-left：半径负数（再往回拉半径的距离就居中了）

​         在`**flex、grid**`布局下`**margin：auto**`会自动占用可用的`空间`，且`**绝对定位**`下，`**auto**`不会起作用

front-size：%是相对于正常字体大小，不是相对于父元素

------

标准流就是元素排版布局中默认从左至右，从上到下的排列方式

------

![img](https://upload-images.jianshu.io/upload_images/6322775-501bf926c3989a1d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![img](https://upload-images.jianshu.io/upload_images/6322775-35173e384eaa7c90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 定位

- 父元素下的子元素是absolute时margin是相对于父元素而言的；一般情况下margin是相对于相邻元素的外边距，left等属性只有在absolute下才能使用
- 在同一个父元素下使用relative，原始位置会自动错开；使用absolute且想保持**贴合**，需要统一**使用一个**定位**基准**，比如都用left和top
- ==Fixed==：绝对定位，是相对于浏览器窗口，**会脱离文档流，滚动时位置不变**；
- ==Absolute==：**绝对**定位，是**相对于最近的父元素**(不是static定位)，**会脱离文档流**，**无视padding**；
- ==Relative==：**相对**定位(偏移)，**不会脱离**文档流，**原来的位置依然被保留**(原来的位置指所有元素的默认叠加位置，比如第二个块元素的原始位置在第一个块元素的下面，那么relative就是从第一个块元素下作为起始位置)。
- （new）==Sticky==(粘性的)：relative和fixed的结合体，通过top等属性，可以设置元素在**距离窗口**多少距离时**滚动时位置不变**
  - tips：可以做到类似element ui的固定表头和列的效果

- padding可以限制内部宽高；

![img](https://upload-images.jianshu.io/upload_images/6322775-4d3009044c7807f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center/>箭头代表正值移动方向，黑色表示可移动，即是说margin-left负值是元素往左移动，而margin-right正值是父元素往右移动

- [padding]()和[margin]()用==百分比==，都是==相对于父元素==

- 小技巧：当把自身体积压缩到0时，就不会浮动了；用**margin-right**是**右侧元素压缩自己**，到0的时候就变成了没有体积的东西，会跟到左侧元素的后面；用**margin-left**是**自己压缩左侧元素**，会取代左侧元素被压缩掉的宽度。

- 总结：margin-left压缩别人，margin-right压缩自己。

- **仅**当父元素**宽高不固定**时，**margin**会**撑大**父容器。

- 小技巧：用（==父元素==的宽度/高度—==子元素==的宽度/高度）÷ 2 = 子元素==居中==时到父元素的==左边距==。
  - 原理：居中即意味着子元素在父元素中左右边距相等，所以用父元素总长/高➖子元素宽高再÷2，就是左右边距的长度


------

边框角：用border-left等属性制作小正方形，通过absolute覆盖到边框角落

## 选择器：

- “aaa bbb”：[后代选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Descendant_combinator)
- “aaa > bbb”：[子代选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Child_combinator)
- “h1 + p”：[相邻兄弟选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Adjacent_sibling_combinator)
- “h1 ~ p”：[通用兄弟选择器](https://developer.mozilla.org/zh-CN/docs/Web/CSS/General_sibling_combinator)
- before伪元素插入图片：content设为空，display必须为block或者inline-block，宽高必须设置才能显示出来；background-repeat以及vertical-align、background-position可以辅助设置图标位置
- 伪类**选择器后**还可以**跟子元素**，以及子元素的选择器——跟在选择器后的子元素可以用于限制在`**选择器特定条件**`下元素的样式改变，比如一些元素初始样式是隐藏，在`**:hover**`悬停时再显示这个元素
- **选择器后**还可以**跟选择器**，但是要注意**顺序**。
- 伪元素**默认**属性是“**内联元素**”，要想设置宽高，必须设置display：block；在设置**背景图片**时，必须设置**content：‘ ’**。
- 属性选择器：
  - “[标签属性]”搜寻所有标签中特定的**属性名**；
- css函数：attr(属性名)——返回选择器前元素的属性值
- nth-child(x)序号**从1开始**，匹配父级元素下==1开始==的**同类元素集合**下的第x个元素
- css选择器**“>”**是仅作用于**儿子**标签，不会作用于下一级标签（如果孙子节点才是要找的标签，使用>是找不到的）
- TIps：
  - css选择器，使用`**逗号，**`间隔应用多个样式
  - css本质上是通过“各类选择器”**定位**标签位置，来设置样式，所以不论是通过 属性选择器、class、id，都是为了一个目的——找到这个标签元素
  - **指定类名等**：**nth-child(n)**是当前元素的·**父级元素**·下的第n个元素，·**只会**·根据指定类名等来·**确定父级**·

------

用opacity设置透明度会影响到子元素，字体也会变透明

------

box-sizing属性：border-box下相当于把padding和border算在content里，盒模型会自动根据padding和border的值来调整content的值

------

width和height属性是相对于包含它的块元素

------

object-fit一般用于img和video标签，当窗口大小改变，不想把图片压扁时，可以用此属性等比例裁剪或者缩放图片

对于横贯整个页面的img，使用object-fit会适得其反

object-fit仅适用于图片上有文字不适宜拉伸的时候

------

输入框有默认选中样式，使用outline：none清除，再设置自己的样式;

如果要选中样式和不选中样式一样，则不需要设置：focus样式

------

**阴影效果：**

  **text-shadow**：水平偏移量 垂直偏移量 距离 颜色。给文字添加发光效果

​    Tips：

​      1、要添加多层阴影，不然颜色会很淡

  **box-shadow**：水平偏移量 垂直偏移量 模糊距离 阴影大小 阴影颜色 内/外

​    Tips：外阴影的时候是“右、下为正”，内阴影的时候是“左、上为正”，**模糊距离是数字越大边缘越模糊**

------

**font-size**百分比是**相对于自身**，而不是父元素

------

## 图层层级z-index

- z-index仅在设置position时有效
- 子元素是否被遮盖取决于父元素跟兄弟元素之间的层级，如果父元素被遮盖，子元素设置再大也没用
- `z-index`==相同==时，HTML结构中后面的元素会遮盖前面的元素。
  - Tips：利用这点可以实现不用改变z-index，只需要`v-for`遍历显示，就能实现弹窗层叠


------

最外层父元素的overflow也会影响里层子元素的显示，超出设置overflow的父元素会被隐藏

------

实现图片和文字重叠：图片只设置宽高，文字容器设置同样的宽高，并用绝对定位left:0和top:0，使用line-height居中

------

**清除input默认样式：**

​    background：默认背景白色

​    border：默认边框

​    outline：默认选中颜色，设为0则清除默认设置

------

图片默认可拖拽，要阻止默认设置，可以用“user-drag：none”

------

text-overflow：必须搭配overflow：hidden（溢出内容隐藏）和white-space：nowrap（强制文本一行显示）

------

没设置宽高的元素，通过同时设置上下边距，可以间接设置宽高

------

**@keyframes关键字**：

  0%和100%可以用from to代替(但其实不太好用，不能来回)；

  搭配animation-name调用定义好的动画名

  animation是自动触发的动画，并且有帧的概念，而transition（过渡动画）和transform（变形）只有开始和结束状态，并且需要依靠条件（hover等）触发;

  transform-origin是相对于自身

**animation动画：**

  animation：以下所有属性的集合，书写位置不限制，例如`animation: test 1s alternate linear 3;`，表示`使用动画test，动画持续时间1秒，交替播放，线性播放，动画播放3次`

  animation-duration：动画持续时间

  animation-timing-function：动画展示速度(先快后慢或是中间快两边慢等)

  animation-delay：动画启动前延迟

  animation-iteration-count：动画循环次数

  animation-direction：动画**正向**播放或者**反**着播放，且当动画播放**不止一次**时，选择动画**轮流**方式(交替反向动画等)

  ※animation-fill-mode：动画播放完是默认清除动画过程中的样式的，这个属性可以设置动画结束时样式是否保留

  animation-play-state：动画暂停或播放，常用于JS中

------

进行3D旋转的时候一定要给每个面设置绝对定位

------

**cursor**：not-allowed和no-drop是**禁用**样式

------

父级没设宽高，自动撑大，子元素宽高百分比会自动往上一级找

------

table标签消除默认间距：`border-collapse:collapse;`