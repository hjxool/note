## Tips：

1. 父元素未设宽高，==子元素百分比无用==
   - 因此祖父元素只设置了==max/min-width/height==时，父元素设置==百分比==最大宽高**是相对于==自身被撑大的体积==**，因此设置`max-height:100%`并没有想要的效果
2. img和grid、flex相性不好，要用“div”将“img”包着才行
3. calc(xxx +/- xxx)中间必须有空格
4. 滚动显示时想要平滑跳转可以使用`scroll-behavior: smooth(平滑)`属性
5. `border-radius`
   - 当元素宽高一样时，`px`超过宽/高度的一半，或者`50%`，就会是圆
   - 但如果是矩形，`px`最多只能使四个角变圆，但是最长的变不受影响，而如果是`%`则会变成椭圆
     - 总结：`px`和`%`效果是不一样的
6. ※**滚动显示**时，**主要**是用**外层父容器**来**限制**，从而使内部滚动容器**受限**从而开始滚动
7. `display: none`
   - 元素不会占用grid/flex布局位置
   - 虽然脱离文档流，但在CSS选择器中依然存在
8. **padding**和**滚动条**位置**仅在**`box-sizing:border-box`时才重叠！
   - 默认盒模型下，滚动条在`content`区域
   - 想隐藏滚动条样式用`::-webkit-scrollbar{display:none}`
9. 经验1：容器用布局样式，不要使用对齐方式，只设**宽度**，**子元素**用**margin**、**padding**设置元素大小，将容器撑起。另外**子元素内**始终都是要用布局对齐的。(但有一种特例，当子元素内有东西是忽隐忽现的，子元素大小会变化，所以还是得设置高度，因为父容器没有高度，是子元素撑起来的)
10. 元素排列方向既是**主轴**，而**交叉轴**默认都是**拉伸**的。在不用布局时，主轴是垂直往下走的，所以只设置高度都可以横向铺满。用**flex**时，主轴是横向的，所以只设置宽度，就可以纵向铺满。只有**grid**相较特殊，里面的元素横纵都是满铺
11. 边框角：用border-left等属性制作小正方形，通过absolute覆盖到边框角落
12. 用opacity设置透明度会影响到子元素，字体也会变透明
13. object-fit一般用于img和video标签，当窗口大小改变，不想把图片压扁时，可以用此属性等比例裁剪或者缩放图片

    - 对于横贯整个页面的img，使用object-fit会适得其反

    - object-fit仅适用于图片上有文字不适宜拉伸的时候
14. 输入框有默认选中样式，使用outline：none清除，再设置自己的样式;

    - 如果要选中样式和不选中样式一样，则不需要设置：focus样式
15. 进行3D旋转的时候一定要给每个面设置绝对定位
16. **cursor**：not-allowed和no-drop是**禁用**样式
17. 父级没设宽高，自动撑大，子元素宽高百分比会自动往上一级找
18. line-height是行高，在一行中字体是默认居中的，当行高与块级元素高度相同，字体就垂直居中了

## float

- **inline-block** 和 **float(浮动布局)**的区别：

- inline-block：整齐，有间隙(可以为父元素添加**{font-size:0}来消除空白符大小**)

- float：参差不齐(需要**子元素高度一致**才会显得整齐)，无间隙。浮动元素依然遵循盒模型。

- 总结：对于横向排列使用**inline-block**可以去除浮动，避免布局混乱；float用于**文字环绕**效果。

- float：使目标元素靠左右放置，父元素设置后不会应用于子元素。子元素设置后，只要没有占满整行，会依次水平排列。父元素应用float布局后，宽度会依照子元素的最大宽度。

- **清除浮动**：之所以会有浮动脱出标准流，是因为应用float的元素**会脱离正常布局**，而位于浮动元素(应用了float的元素)之下的**正常布局内容**会**围绕**浮动元素**，填满右侧区域，**所以要把**clear**添加到**浮动元素之下的正常布局元素**，来清除其两侧的浮动

## flex布局

- 主轴默认**横向**排列

  - 横向排列时，纵向默认铺满父容器，因此当`flex-wrap: wrap`时，出现多行，就会==平分==父容器==高度==

- [order]()：弹性容器下子元素属性，修改排列顺序

  - 无需修改html结构，通过order可以修改元素==排列顺序==。例：`.item{ order:2; }`

- **flex-direction**决定项目排列方向(**想要让大大小小每一个元素自动占一行就用这个属性**)

- **flex-flow**属性是flex-direction(方向)属性和flex-wrap(换行)属性的简写形式

- flex是一种布局方案，应用flex的元素标签自动成为“容器”，容器下有6种属性，分别为方向、换行、方向+换行、横向(主轴)对齐方式、垂直方向(纵轴)对齐方式

- 注意**align-content**属性是**多轴**条件，所以下属的space-around和stretch参数都是针对轴线；stretch占满整个交叉轴，指纵向拉伸项目；space-around指轴线间的间隔而非项目间隔

- 注意，flex所属的元素将成为布局容器，旗下的元素比如<a>标签之类的有另外6种属性设置。分别为：顺序、放大倍数(都是1则平分空间，其中一个是2或者更多则分配的空间是1的2倍或者更多倍，而不是按照自身大小的倍数，注：默认值为0不放大)、缩小比例(注：默认值为1，表示空间足够的情况下不缩小，如果为0则什么情况下都不缩小，而1的项目平分剩下的空间)、基础大小、以及放大，缩小，基础大小的综合属性flex、单项目的对齐方式(可覆盖父容器的align-items，默认值是auto，表示继承父元素的align-items属性)

- flex布局下子元素**不能设置成absolute**,absolute会**脱离**布局控制，**relative**也会**无视**absolute的元素；**非flex布局**下即使用了absolute页面也会自动编排元素，但是设置了定位后就会脱离页面自动编排的位置，所以在设置**left和right**的时候元素不会叠到一起(**上下排布**的元素)，但是用**上下**定位的时候**会堆叠**

- 用flex排布字体会将div元素里的字当作最小的元素块移动位置

  Tips：

  - 使用“**flex-grow**/flex-shrink”可以控制元素自动填充，是flex的自动填充方法，“flex-grow”默认不填充扩大，所以要单独设置
  - [flex-grow]()
    - flex主轴==横向==时，==width==失效
    - flex主轴==纵向==时，==height==失效
    - flex-grow最好搭配==overflow:hidden==，容器自动伸展，因此会被撑大，里面的子元素设置overflow失效
    - 设置==align-items==、==justify-content==会使==flex-grow==失效


  Tips：

  - 只设置align-items==或==justify-content其中一个时，flex-grow会向==未设对齐属性==的地方扩充


## grid布局

- 想要自动填充滚动，用grid-auto-row/column将自动填充的内容设置为自己想要的大小

- `overflow：auto`会在gird设置了`对齐方式(比如align-items、justify-items)`时**失效！**，所以往常能自动滚动是因为，里面没有设置对齐格式，让子元素自动填充网格再配合overflow才实现

- 自动撑大父容器不能使用百分比

- **绝对定位不**会**占用**grid划分好的格子

- 父容器设置了宽高后，想要一行一行自动填充内容，可以用“grid-**template**-columns:repeat(**auto-fill**, **绝对值**);”，会根据容器大小自动填充
  - auto-fill是容器宽高设置的情况下尽可能往里填充元素，对那种宽高自动撑大的容器可以不用设置template-column/row；
  - repeat(auto-fill)在固定大小的父容器中非常好用，会自动换行
  
- **justify/align-content**是指分割块**铺不满**容器时，分割块的定位

- 如果想横向扩充，需要设置grid-auto-flow：column（先列后行），先填充满列再补充行，其实就是变相告诉浏览器是横向排列还是纵向；

- **template-col/row妙用！**用于指定**起始**列/行数，比如auto-flow：row(默认)时，纵向滚动，默认是一行**只有一列**，而使用**template-column:repeat(auto-fill/指定列数，绝对大小)**，可以指定一行有多少列，然后向下滚动

- fr单位是将以知宽度按比例切割，==不能用于撑大父元素！==
  - 所以不能将==repeat(auto-fill)==与==fr==搭配使用
  
  - `grid-template-xx: repeat(num, 1fr)`不能用于滚动显示！例
  
    ```css
    /* 当子元素在5个以内时，会自动根据父容器宽度分割5份，当超过5个时auto-columns
    的大小不会变，但是template-columns的1fr会自动压缩！*/
    .bos{
        display: grid;
    	grid-template-columns: repeat(5, 1fr);
    	grid-auto-flow: column;
    	grid-auto-columns: 6rem;
    	overflow: auto;
    }
  
  - 用`repeat(num,1fr)`加==每个单独元素==设置==最小大小==，就可以得到伸缩自如的小方格
    - 当触发单独元素的最小大小后，`1fr`的元素块会自动以最小宽高排列，撑开父元素
    - 如果是`repeat(num,50px)`这样的绝对大小，当这个绝对大小小于元素块的最小宽高，，元素块不会间隔开来，而是顶出自身分割的小方块，堆叠在一起，从这点说明，grid分隔的块都是以==left-top==为顶点开始填充
  
- `grid-gap`：第一个参数是行间隔，第二个参数是列间隔

- 设了宽高的情况下使用百分比或者绝对大小自动填充看起来是一样的，但是滚动的时候超出原始宽高的部分还是会自动压缩

- 使用**template-row/column**时，元素会自动填充，而不会自动垂直排列。只有超出**template(模板)**的才会，所以才需要**auto-flow:column**来规定**多出来**的元素如何排布

- **默认**会**自动拉伸**，但是当设置align或justify对齐时，会自动将未设宽高的元素压缩至最小；这与**flex**不同，flex只会自动拉伸高度，在direction：column时，则会拉伸**宽度**，因为主轴变为纵轴时，原来的高度就变为了宽度

- [grid-column-start/end]()
  - 分的格子序号==正序==是从1开始，结尾序号是==格子数+1==；也可以==逆序==，从==-1==开始到==负格子数+1==
  - ==start==是选中序号之==后==的内容，==end==是选中序号之==前==的内容
  
- grid-template==划分的网格==，不管是用什么单位划分，都会被==超出所划区域大小的==子元素的==绝对宽高**撑大**==，其他子元素会自动缩小，划分的网格也会变形
  - 因为划分的网格一开始的网格大小就已经确定，所以用==百分比宽高**无法撑大**==

## flex和grid共同点

- 在==弹性布局==下，不区分行内元素和快元素，因此行内元素也可以设置宽高
- ==padding==下会以没被占用的空间进行分割
- grid和flex居中都是以盒模型中的content为准，所以padding会影响居中效果
- 只能应用在==子==元素，不能用到==后代==元素

## 页面布局、元素

- **页面居中**：
  - ==绝对定位==：==top==等属性设为0，`margin:auto`。==会拉伸未设宽高的地方==
    - 此居中方法==不会==在自适应滚动显示时==遮挡元素==
    - ==绝对定位==和==固定定位==，都只能在right时margin-right生效、left时margin-left生效
      - 因为满足等式下，设置right，left就是auto，这时使用margin-left会被left:auto抵消
  - ==相对定位==：==left、right==属性设为0，`margin:auto`。==会拉伸未设宽高的地方==
    - 绝对定位和相对定位都会提升层级高度，谁写在后面谁就在上
  - left：50%（移动边缘），margin-left：半径负数（再往回拉半径的距离就居中了）
    - 会在自适应滚动显示时==遮挡==元素
  - `left:50%;top:50%`再`transform:translate(-50%,-50%)`
    - 滚动显示会遮挡
  - 在[flex]()、[grid]()布局下`margin：auto`会自动占用可用的`空间`
    - ==绝对定位==下，`margin：auto`不会起作用
    - ==padding==没有auto
  - 原理：未设置flex时，元素水平宽度满足等式`父元素宽度 = margin-left + border-left + padding-left + width + padding-right + border-right + margin-right`，有一项为auto时，会自动填充满足等式，因此会出现居中等效果。
    - 设置flex时，元素高度满足对应等式
    - ==绝对定位==的元素等式多两个==left==和==right==，且==纵向高度==也要满足对应等式
- 标准流就是元素排版布局中默认从左至右，从上到下的排列方式
  - **绝对定位**会脱离**文档流**，不会撑大==未设宽高==的父容器
  - 脱离文档流的元素不在满足等式，不再独占一行
- [BFC]()：独立的布局区域
  - 开启BFC的元素和子/父元素外边距不会重叠
  - 通过`overflow、display、float`可以开启
- ==front-size==：==%==是相对于==父元素==字体大小(父元素没设置字体大小则为16px)，不是相对于父元素
- Tips
  - 当子元素大小是==px==，就没必要设置父元素宽高，用==padding==和==margin==撑开并加以边框，尤其==父容器==使用==flex、grid对齐属性==时，更不需要设置宽高，用==padding==将元素边框与内容撑开一个舒服的距离即可
  - ==自动撑大==的元素需要==设置overflow==，不然撑大到边界时不会出现滚动条，而是会继续顶出元素边界

## 定位

- 父元素下的子元素是absolute时margin是相对于父元素而言的；一般情况下margin是相对于相邻元素的外边距，left等属性只有在absolute下才能使用
- 在同一个父元素下使用relative，原始位置会自动错开；使用absolute且想保持**贴合**，需要统一**使用一个**定位**基准**，比如都用left和top
- ==Fixed==：绝对定位，是相对于浏览器窗口，**会脱离文档流，滚动时位置不变**；
  - **但**并不是悬浮在所有元素之上，依然会被图层高度高于它的覆盖，如果在同一父容器下，会被==z-index==更高的兄弟元素覆盖
  - 宽高也是相对于视窗
  - 虽然==定位==是相对视窗，**但是**从DOM树上来说还是属于父节点，因此，==父容器==中的==fixed元素==依然会==事件冒泡==到外层父容器
  - 不会撑开祖、父容器，即不会出现滚动条
- ==Absolute==：**绝对**定位，是**相对于最近的父元素**(不是static定位)，**会脱离文档流**，**无视padding**
  - 绝对定位会==脱离文档流==，即==不会撑开父容器==
  - 因为==无视padding==，所以==子元素==宽高百分比是相对于父元素==border内的大小==
    - ==top==等定位属性也是相对于==border==
  - 虽然脱离了文档流，但是==默认==会排在文档流中==兄弟元素的后面==，而不是覆盖
    - 但如果绝对定位元素在兄弟元素前，则会覆盖其上，因为兄弟元素会无视脱离文档流的元素
- ==Relative==：**相对**定位(偏移)，**不会脱离**文档流，**原来的位置依然被保留**(原来的位置指所有元素的默认叠加位置，比如第二个块元素的原始位置在第一个块元素的下面，那么relative就是从第一个块元素下作为起始位置)。
  - ==top==等定位属性相对于==content==
- ==Sticky==(粘性的)：relative和fixed的结合体，通过top等属性设置==阈值==，表示元素在**相对父元素**多少距离时**变成fixed**
  - 可以做到类似element ui的固定表头和列的效果
  - ==不会脱离==文档流！即还是遵循块元素铺满一行等规则
  - ==不需要父元素设置position属性==

## 选择器相关

- before伪元素插入图片：content设为空，display必须为block或者inline-block，宽高必须设置才能显示出来；background-repeat以及vertical-align、background-position可以辅助设置图标位置

- 伪类**选择器后**还可以**跟子元素**，以及子元素的选择器——跟在选择器后的子元素可以用于限制在`选择器特定条件`下元素的样式改变，比如一些元素初始样式是隐藏，在`:hover`悬停时再显示这个元素
  - 伪类选择器仅可以用==兄弟/子元素==选择器实现特定条件下用CSS切换显示元素

- **选择器后**还可以**跟选择器**，但是要注意**顺序**。

- 属性选择器：
  - “[标签属性]”搜寻所有标签中特定的**属性名**；
  
- ==css函数==

  - [attr(属性名)]()——返回选择器前元素的属性值
  
- TIps：
  - css选择器，使用`,`间隔应用多个样式
  - css本质上是通过“各类选择器”**定位**标签位置，来设置样式，所以不论是通过 属性选择器、class、id，都是为了一个目的——找到这个标签元素
  - **指定类名等**：**nth-child(n)**是当前元素的**父级元素**下的第n个元素，**只会**根据指定类名等来**确定父级**
  - `.head{}  .box1 > .head{}`==会结合==**外部**样式和**子元素**样式
  
- [伪元素]()：表示一个==特殊的位置==

  - 并不真正存在于dom节点中，无法通过JS获取和操作。

  - 默认是`display:inline`，即使设置宽高也无法显示。所以必须设置成==块级元素==，`inline-block`或者`block`
  - 必须设置`content`属性才能显示
  - [::selection]()：选中的==文字==设置样式
  
- [伪类]()：表示一个==特殊的状态==。形如`li:first-child`
  - `ul > li:first-child`正确的解读方式应该是==当作交集==选择器。即，是li且是父元素下的第一个==子元素==的才会被渲染，比如li前有其他标签，那first-child就不会生效
  - [nth-child(n)]()：匹配==父级==元素下==1开始==的下的第n个元素
    - 序号**从1开始**
    - ==n==的范围是==0==到正无穷
  - [nth-of-type(n)]()：==同类型==里的第n个
    - 假如不同类型用==同一个calss==，`.bbb:nth-of-type`，那么会==各自计算==自己类型的第n个
  - [:not(:nth-child(2n))]()：除了xxx条件的。
  - 必须是==同一父元素==下
  
- ==交集==选择器：同时满足多个条件。`选择器1选择器2...`

- ==并集==选择器：同时选择多个对应元素。`选择器1，选择器2，...`

- ==子元素==选择器：仅找最近一层的==所有==子元素。`.aaa > .bbb`

- ==后代==选择器：只要是包含关系的元素都符合。`.aaa .bbb`

- ==兄弟==选择器：==紧挨==着的下==一个==兄弟元素。`.aaa + .bbb`

  ```css
  /* elementUI的按钮用的样式，表示从第二个同级兄弟节点开始左边距 */
  .el-button + .el-button {
      margin-left: 15px;
  }
  /* 注:兄弟选择器表示的是 同级 下选择 因此不需要重复上级选择器 */
  .form > .button + .button {}
  ```

- ==所有兄弟==选择器：所选元素==之后==的所有兄弟元素。`.aaa ~ .bbb`

- ==标签属性==选择器：带有所选择属性的==所有==元素。
  - `[title]`
  - `[title=xxx]`这种新式不用给xxx加引号，只选择属性值特定的元素
    - 等号前可以加正则符号`[title$=abc]`

- ==属性继承==inherited：==除了==影响==布局==的==通用==属性都会继承。例，`font-size、color`

- ==优先级==：!important 关键词> 内联 > id选择器 > Class|属性|伪类 > 元素选择器 > 继承(inherited)。计算方式是根据左大右小以及累加
  - 当优先级==相同==，有同名属性冲突，以在==CSS文件中后声明==的为准

## 阴影效果

- [text-shadow]()：水平偏移量 垂直偏移量 距离 颜色。给文字添加发光效果
  - 要添加多层阴影，不然颜色会很淡
  
- [box-shadow]()：水平偏移量 垂直偏移量 模糊距离 阴影大小 阴影颜色 内/外
  - 外阴影的时候是“向右、下为正”，内阴影的时候是“向左、上为正”
  - ==模糊距离==是数字越大==边缘==越模糊
  - 也可以是==水平偏移 垂直偏移 模糊距离 阴影颜色 内/外==
  - 多个叠加的阴影可以更==亮==，叠加阴影用逗号分隔，例：`0 0 10px blue,0 0 20px blue,...`
    - 原理是模糊距离会使阴影整体都很淡，且边缘处圆滑，多个阴影叠在一起，相同距离的位置颜色会更深

## 图层层级z-index

- z-index仅在设置position时有效
- 子元素是否被遮盖取决于父元素跟兄弟元素之间的层级，如果父元素被遮盖，子元素设置再大也没用
- `z-index`==相同==时，HTML结构中后面的元素会遮盖前面的元素。
  - Tips：利用这点可以实现不用改变z-index，只需要`v-for`遍历显示，就能实现弹窗层叠

## 动画、过渡、变形

- `@keyframe`
  - 0%和100%可以用from to代替(但其实不太好用，不能来回)；

  - 搭配animation-name调用定义好的动画名

  - animation是自动触发的动画，并且有帧的概念，而transition（过渡动画）和transform（变形）只有开始和结束状态，并且需要依靠条件（hover等）触发;

  - transform-origin是相对于自身

  - animation动画：
    - animation：以下所有属性的集合，书写位置不限制，例如`animation: test 1s alternate linear 3;`，表示`使用动画test，动画持续时间1秒，交替播放，线性播放，动画播放3次`
    - animation-duration：动画持续时间
    - animation-timing-function：动画展示速度(先快后慢或是中间快两边慢等)
    - animation-delay：动画启动前延迟
    - animation-iteration-count：动画循环次数
    - animation-direction：动画**正向**播放或者**反**着播放，且当动画播放**不止一次**时，选择动画**轮流**方式(交替反向动画等)
    - ※animation-fill-mode：动画播放完是默认清除动画过程中的样式的，这个属性可以设置动画结束时样式是否保留
    - animation-play-state：动画暂停或播放，常用于JS中

  - 无法用==duration==控制==从top：0变化到bottom：0==这样的属性值切换
- transition
  - 只能对有效数值进行过渡，比如：`0到100px`等，但是值由浏览器自动计算的auto无法过渡，例如：从top：0到bottom：0，浏览器不知道怎么将bottom：auto过渡到bottom：0
  - [transition-timing-function]()：
    - [steps(帧)]()：里面填入参数只能是正整数。表示过渡效果由几帧来完成
- transform
  - 只针对元素本身(即变形的大小百分比等都是相对自身)，变形不会脱离文档流，也不会挤压其他元素
  - 可以同时写多个变形，中间用空格隔开`transform:translateX(-50%) translateY(-50%)`
  - [perspective]()：视距。默认远近大小都相同，但设置了视距，移动Z轴会使元素==变大/小==。`transform:perspective(800px) translateZ(500px)`，视距必须写前面才会生效。
    - 设置视距很有用！除了像scale放大缩小元素，最主要的是在==旋转==时产生==透视==效果！
    - 也可以给==变形的元素父级==加上perspective属性

  - [transform-style]()：默认值flat，2D平面效果。可选值，==preserve-3d==3D效果
  - `transform-origin`：以某点为圆心旋转
    - 默认元素左上角`0,0`点
    - `transform-origin: center`：以元素中心点为圆心旋转
  - `transform: rotate(角度)`：以圆心为基点旋转一定角度
    - 角度为负：逆时针旋转
    - 角度为正：顺时针旋转

## table

- table标签消除默认间距：`border-collapse:collapse;`

## 盒模型

- [border-box]()：
  - `border + padding + content = css设置大小`
  - 仅在`border-box`下才能做出==三角形==，通过将一边的`border`属性拉长>=宽/高，再将两侧的border拉长到宽/高的一半
    - 注！`border-left: 100% solid;`不能用==百分比==
- padding可以限制内部宽高
- ![img](https://upload-images.jianshu.io/upload_images/6322775-4d3009044c7807f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<center/>箭头代表正值移动方向，黑色表示可移动，即是说margin-left负值是元素往左移动，而margin-right正值是父元素往右移动

- [padding]()用==百分比==，是==相对于父元素==
- [margin]()
  - 当把自身体积压缩到0时，就不会浮动了；用**margin-right**是**右侧元素压缩自己**，到0的时候就变成了没有体积的东西，会跟到左侧元素的后面；用**margin-left**是**自己压缩左侧元素**，会取代左侧元素被压缩掉的宽度
  - ==margin-left/top==影响==自己==，==margin-right/bottom==影响==别人==
  - **仅**当父元素**宽高不固定**时，**margin**会**撑大**父容器
  - 父元素与子元素==上边距相邻==，**且**父元素==没有设置边框==进行隔开，子元素设置margin会传递给父元素！父元素也会向下移动
- inline（==行内元素==）
  - 不能更改height，width值，大小只能由==内容决定==
  - 不会独占一行
  - 可以使用padding全部属性以及margin==左右==属性。
  - ==垂直==方向的margin、padding、border属性都==不生效==
  - **但是**！==脱离文档流==后就可以设置宽高即以前不生效的属性
    - [float]()、==绝对定位==均可脱离文档流
      - ==float==脱离文档流不会遮盖前一个兄弟元素，但是==绝对定位==会堆叠遮盖
- block（块元素）
  - 可以更改height，width值以及padding、margin全部属性；但是会独占一行；默认填满父元素宽度
- inline-block（行内块元素）：不独占一行的块元素。
- Tips：
  - 用（==父元素==的宽度/高度—==子元素==的宽度/高度）÷ 2 = 子元素==居中==时到父元素的==左边距==。
    - 原理：居中即意味着子元素在父元素中左右边距相等，所以用父元素总长/高➖子元素宽高再÷2，就是左右边距的长度
  - ==虚线样式==：不设置内容大小，只设置边框宽度，除了==dotted==，==虚线==和==实线==都可以合并宽度。使整个元素呈现虚线的样式
  - ==margin坍塌==：指==垂直相邻==两块级元素，使用垂直方向的margin会取相邻边margin的较大值，而不是求和
    - 只有==块元素==会坍缩，flex等弹性布局下不会
  - box-sizing：属性。border-box下相当于把padding和border算在content里，盒模型会自动根据padding和border的值来调整content的值
  - ==背景色==会给除了==margin内的所有区域==都上色，使用`background-clip:content-box`可以将边框底部的背景色去除
  - 没设置宽高的元素，通过同时设置上下边距，可以间接设置宽高

## 文字

- 文字始终处于行高最==中间==
- 文字有字体框的概念，`font-size:50px`即==行高==/==文字框==高度是50px，文字是小于50px的
- [line-height]()
  - 可以设置1、2这样的数字，表示几倍的==font-size==
  - 仅表示单行的行高
- [text-align]()：文字的==水平对齐==。可选值：left、right、center、justify
- [vertical-align]()：文字==垂直==方向对齐。可选值：baseline(基线)、top、bottom、middle、px
- [white-space]()：网页如何处理空白。可选值：normal、wrap、==pre(保留空白，可作为预处理格式)==
  - 文字省略：需要具备几个条件，宽度、空白不换行、超出部分裁剪、文字溢出如何处理
- `text-overflow`
  - 必须搭配`overflow：hidden`(溢出内容隐藏)和`white-space：nowrap`(强制文本一行显示)


## 背景

- [background-position]()：九宫格形式，必须由两个值指定位置
- [background-size]()：设置背景图大小==以及图片适应方式==。例：`background-size:cover/contain`
- [background-attachment]()：默认scroll，即背景图随元素移动。也可以用fixed。滚动时背景图相对于==视窗==固定不动

## RGB颜色

- ==rgb/rgba==：0~255之间的数，可以用==百分比==！如：`rgb(10%,20%,30%)`
- ==十六进制==：==#红色绿色蓝色==。00~ff之间的数。如：`#aabbcc`
- ==HSL==：用于**JS**计算呈色
  - H：色相。0~360的值。用于计算调色
  - S：饱和度。0%~100%。可以理解为“灰度”，0%全是灰色，50%颜色不饱和，一般取100%
  - L：亮度。0%~100%。0%全是黑色，100%全是白色，一般取50%

## Less

- ```less
  >符等同css
  .box{
      >.子{}			生成css		.box > 子{}
      .后代{}
  }
  &符不只可以跟选择器 还可以直接用于拼接字符串
  .box{
      &box2{}			生成css		.boxbox2{}
  }
  ※混入：将其他类的样式复制进另一个类，达到样式的复用，而不用复制粘贴，仅能使用class样式
  .box{
      .box2()			生成css		.box{ 里面是.box2的样式 } 不能是子元素！只能是**后代**元素样式
      >.box3()		不行
      .box3()			box3是>子元素也不行
  }
  ```

## 移动端

- 默认情况下，移动端视口宽度都为980px(css像素)
- 正常情况，物理像素和css像素为1：1，当浏览器放大或操作系统放大显示时，比例变为==放大/缩小比：1==
- [<meta>]()标签：定义元数据
  - name：属性。可选值之一==viewport==是视口，在==content属性==中设置`content="width=375px"`width，可以设置固定视口宽度，也可以通过[device-width]()这一浏览器给出的变量获取当前设备视口宽度
- ※==移动端自适应==
  - 第一种方法(==vw==)
    - 根据设计图上移动端像素比例，如`40px(元素大小) ÷ 390px(视口像素) × 100 = 10.25vw`，用[vw]()设置元素元素自适应大小
  - 第二种方法(==rem==)
    - 在`html{ font-size: 5.333vw; }`中设置根节点字体大小
    - **但是**大小不是用px这种绝对单位，而是设置为相对视口的单位vw，由`100vw = 390px(视口/CSS像素)`得`0.2564vw = 1px`，**但是**font-size最小值为12px，为了方便计算，设置为20px，即==5.128vw==，这样，根节点字体大小就是自适应
    - 设置元素大小为40px，即==10.256rem==
    - 这样在不同移动端屏幕下，会自适应缩放

## 媒体查询media

- 一般来说，==视口以**宽度**为准，不考虑高度==，因为大多数情况下，都是横向满铺，纵向滚动显示
- 语法`@media 媒体类型、特性组成的规则{}`
  - 媒体类型：`@media screen`
    - all所有设备
    - screen带屏幕的设备
    - print打印设备
    - speech屏幕阅读器
  - 媒体特性：`@media (width:500px)`
    -  可选项：width、height、==min-width==(最**小**宽度)、==max-width==(最大宽度)等
    -  用括号圈起来
    -  不要用==逗号==，表示==或==的关系。用==and==，表示==与==
       -  `@medai screen and (min-width:500px) and (max-width:1200px)`

## 引入外部样式表

- 语法
  - `@import 'path.css'`
  - `@import url('path.css')`

## 段落前加空格

- 使用`:before`

  ```css
  .text::before{
      content: '';
      /* 这个很重要，既不会像block独占一行也可以显示为块 */
      display: inline-block;
      width: 50px;
  }

## input样式修改

- 清除input默认样式
  - `background`：默认背景白色
  - `border`：默认边框
  - `outline`：默认选中颜色，设为0则清除默认设置

## video样式修改

```css
/* 隐藏进度条 */
video::-webkit-media-controls-timeline {
	display: none;
}
/* 隐藏播放按钮 */
video::-webkit-media-controls-play-button {
	display: none;
}
/* 隐藏全屏按钮 */
video::-webkit-media-controls-fullscreen-button {
	display: none;
}
/* 隐藏当前播放时间 */
video::-webkit-media-controls-current-time-display {
	display: none;
}
/* 隐藏剩余时间 */
video::-webkit-media-controls-time-remaining-display {
	display: none;
}
/* 隐藏音量按钮 */
video::-webkit-media-controls-mute-button {
	display: none;
}
/* 音量控制条 */
video::-webkit-media-controls-volume-slider {
	display: none;
}
/* 所有控件 */
video::-webkit-media-controls-enclosure {
	display: none;
}
```

## 首行加缩进

```css
p::first-letter {
    margin-left: 20px;
}
```

## 导入字体包

- 使用`@font-face`

  - `format('woff2')`作用是指定所引入字体文件的格式类型，帮助浏览器正确地识别和处理字体文件

  ```css
  @font-face {
      font-family: '自定义名称';
      src: url('路径/font.woff') format('woff'), // 如果有多个 这里是，号
           url('路径/font.woff2') format('woff2'); // 结尾要用；号
  }
  // 使用导入的字体
  .xxx {
      font-family: '自定义名称', '字体包中字体名称';
  }
  ```

- 支持的格式
  - `woff2`：目前最常用的字体格式，具有高压缩率和广泛的浏览器支持
  - `woff`：`woff2`的前身
  - `ttf`：几乎所有的==操作系统==和浏览器都支持
  - `otf`：`ttf`的升级版，具有更强的功能和更广泛的语言支持
  - `eot`：主要用于==旧版浏览器==
