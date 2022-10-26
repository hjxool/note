## Tips：

- 修改element样式必须把element样式文件引入时放前面
- element默认**横向铺满**父容器，所以要在使用element的地方用一个容器包裹起来限制宽度，**但是**element有默认高度(绝对大小)，并不会铺满整个父容器
- 二次封装就是“vue.component{}”中写入element ui等其他组件，并用**“prop”**属性从component外传入element ui元素内，“原生事件”还是写在组件内
- 目前element ui不支持Vue3
- element搭配自适应效果并不好，因为**内部**都有最小的**绝对尺寸**，比如padding之类的，扩大还行，但如果小到一定程度，就无法达到很小的效果了，像”input输入框“之类的**自动填充组件**可以被”**父容器**“压缩，但是“button按钮”之类**本身就不大的组件**，靠“父容器”**无法**进行**压缩**
- element组件的方法传入参数时，必须用`或者v-bind`
- 给elementUI绑定自定义事件是不行的，必须要事件后加“**.native**”属性
- 使用自定义icon：必须用`before下的content`占位，并将文字隐藏才能显示出来自定义图标
- element中的label属性是标签对应的值，如果标签后无文字labels属性就充当文字
- 对于需要转换的标识符使用**“:formatter”**函数，其中默认传递的第一个参数**“row”**包含对象数据，第二个参数**“column”**包含element自己定义的一些属性方法(一般用不到)
- 插入单个元素用slot即可，但如果一个单元格内有多个元素标签呢？使用**“<template slot-scope='scope(固定名称)'”**，表示一个范围域；他可以获取到表格每一行数据，scope包含“row（每一行的数据）”、“column”、“$index（每一行索引值）”、“store（table 内部的状态管理）”
- 需要用`this.$refs.xxx.$refs.input`才能修改到elementUI的控件节点
- [形参$event]()：可以==改变==组件传入的==第一个默认值==**位置**，如果没写$event，且传入了自定义数据，传入值会按照形参顺序依次覆盖默认值


## 树形控件：

- 每个节点都是.el-tree-node，如果其下还有子节点则用.el-tree-node__children包裹.el-tree-node

- 节点前加样式的关键点在于.el-tree-node的after和before的**大小**以及**top**属性，构造一个横向线和竖向线（注意节点本身是并排的，所以要给节点和子节点都加padding-left使其错落开）

- 当使用==自定义键名==，需要用[:props]()配置项

  ```js
  :props="setting"
  ...
  setting:{
      label:'name',
      children:'child'
  }
  ```

- [slot-scope=“{node，data}”]()：==node==是element节点的一些属性、==data==则是自己定义的数据

## form表单

### 表单验证

- 使用==rules==时必须要搭配==model==，不然会出错

- ==无法验证v-for遍历的同名字段==

- 表单要用`v-if`渲染，不然重置后的表单项会一直进行验证，一点开就报错

- ==prop="xxx"==验证时xxx必须要跟==v-model中的值相同==

- 在表单**头部**用“:rules”接收所有规则，在**表单项**用“prop”接收“:rules”中的“单个规则(数组)”，“单个规则”又是一个数组，其中由**多个对象**表示**多条规则**

- 使用:rule="rule"绑定验证规则时，如果是使用element提供的方法一次只能验证一个条件，例如

  - `{required: true, message: '年龄不能为空'}，

  - {type: 'number', message: '年龄必须为数字值'}`

- 多个条件就只能用多个对象表示；

- 一行有多个项要验证时，必须每一项用`<el-form-item prop="date">`包裹项

  - `<el-form-item>`不写`label`则不会有label的空位，不用担心会多出来空白

- 验证==时间/日期、数组类型==的数据时，验证规则要加`xxx:[{type:'date/array'}]`属性

- ==提交==时，要再次对表单验证使用==表单元素方法==，通过`ref`等获取到节点，用`validate((result)=>{})`方法统一对表单验证，参数是==回调函数==，回调函数收到两个参数：校验结果、未通过校验字段

- 但如果用自定义函数验证，则需要用关键词**'validator'**，

  - `{ validator: func, trigger: 'blur' }`

  - ==func==必须写在vue的==data()函数==中，在==return外面==

    ```js
    data(){
        let func = (rule，value，callback)=>{
            if（value==条件）{
                代码
            }else{
                callback(new error('错误提示'))
            }
        }
        return{
            data数据
        }
    }
    ```

  - [基本规则]()：关键词

    ```js
    requured:true表示字段必填
    pattern:正则表达式
    min、max:最大最小字符串长度
    trigger:触发方式
    ```

### el-form-item：

- 会继承el-form的label-width；但**嵌套**在 el-form-item 中的 el-form-item 标签宽度默认为零，**不会继承** el-form 的 label-width；
- ==label==中想==自定义图形==时，使用==十进制==Unicode编码。
  - 可以理解为label中的字符串会原样搬入HTML，所以要用HTML的Unicode编码
  - 如果删除label属性或者空字符串，element会将label位置屏蔽

- ==required==属性只控制==✳==的显示，并不能影响样式
- ==el-form-item==上写==prop==，标签内可以包裹==多层结构==，**只要**里面是==单个==需要验证的内容，就可以生效

  Tips：

  - 即使里面包裹不是单个el组件，也只会验证prop后跟的那个属性，**但是**prop验证==视觉效果==生效于==el-form-item==标签==包裹的所有部分==

## 单选/多选

- 单选
  - `v-model`表示值，`<el-checkbox/>显示内容`
  - [el-radio-group]()：包裹单选，就可以用label切换，在同时显示的情况下知道你点的是哪一个，radio靠label传值，如果没有设置标签内显示的文字，则默认显示label内容
  - ![el-radio-button](https://upload-images.jianshu.io/upload_images/6322775-77fd45a188be6b26.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)el-radio-button
  
- 多选：
  - `:label=""`表示勾选的值，不是`:value`！
  - 多选用==checkbox-group==包裹时，==v-model==值必须初始化为==数组==
  - 多选==垂直排布==：`<el-checkbox style="display:flex;align-items:center;">`，并用`<el-checkbox><div>包裹内容</div></el-checkbox>`


## el-option：

  label是用于显示于页面的内容，value是选中后传递给后端的值，`el-select` 中的 `v-model` 绑定传递给后端的值；

  如果少了 `label` `el-select` 显示的值会变成 `v-model` 中的值

## 按钮：

  用·**<el-button/>**·是必定有按钮边框的，要想没有边框可以使用·**<i class="图片地址" />**·

  ·**<i class="图片地址" />**·大小和颜色由·**font-size**·和·**color**·决定，可以把它视作异形字体

## 时间/日期选择器

- [el-time-select]()：是精确到**分钟**的时间选择器
  - `picker-options`格式只能是`start：xxx，step：xxx，end：xxx`
  - 得到的输入结果是==字符串==
  - ==可以传入==Date对象，但是==没有意义==，只有显示的时候会自动==toString==，但是得到的输入依然是Date对象

- [el-time-picker]()：精确到**秒**。
  - `picker-options`格式只能是`selectableRange: '18:30:00 - 20:30:00'`
  - 得到的输入结果是==Date对象==
  - ==可以传入==Date对象，显示的是==时分秒==

- [日期]()选择器==接收参数==可以是==字符串==和==Date==对象，字符串只要不是连在一起的日期，都能识别
  - 但是==onchange事件==获得的参数都是==Date对象==，不是==字符串！==
  - 得到的输入是==Date对象==
  - 选择的是一个时间范围的话，`v-model`得到的是一个==数组==！数组第一位是开始，第二位是结束
- [日期时间选择器]()：得到的也是==Date对象==
- [picker-options]()属性中配置选项
  - 接收的是一个对象
  - 对象中[disabledDate]()方法形参是==每一天时间==，必须有==return true/false==，==返回true则禁用！==


## select选择器

- 当==value==是==对象==时，就不再需要用`:key="xxx.id"`来绑定key了，必须要使用`value-key`(注意此处是[自动取所选对象的==第一层键值==]())

  - ```js
    数据：[{id:1,value:'asdas'},{id:2,value:'123'}]
    HTML：<el-select v-model="xxx" value-key="id"/>
    ```

## 表格

- **table**的**列标签**可以加**“class-name”**属性添加样式
  - table标签下**添加样式**有**xxx-class-name:"className"**和**:xxx-class-name:"function({row,index}){if(index==n){return 'className}}"**两种形式
- 使用`:formatter="函数(row,col)"`属性方法格式化传入值和表格显示内容，方法中用`return`返回显示在页面的内容
- 取表格中行数据
  - 对`<el-table-column>`，必须要用特有的属性方法才能获取到行数据
  - 或者是`<el-table-column>`中插入`<template slot-scope="scope">`，`<template>`包裹中的标签都可以通过`scope.row.属性名`传入或调用值进行判断
- 表格内添加自定义内容样式，只需要用`<template slot-scope="scope">`包裹自定义内容即可
- 勾选
  - 最重要的是[toggleRowSelection(行对象,true/false)]()，此方法会触发==selection-change事件==，用以控制勾选项是否符合条件要被勾选

- 修改表格样式
  - 背景色
    - 必须同时修改`<el-table>`标签，以及`.el-table tr`样式的==background==颜色
  - 文字颜色
    - 修改`<el-table>`标签的==color==，以及`.el-table thead`的color
  - 鼠标悬浮
    - 背景色：修改`.el-table--enable-row-hover .el-table__body tr:hover>td`
  - 边框
    - 修改`<el-table>`的边框样式，以及==单元格==`.el-table td`和`.el-table th`里的`border-right border-bottom`属性

## Tabs标签页

- 点击选项==name必须==是==字符串==，因此如果是遍历用索引当分页切换，必须要`:name="index+''"`，将数字转换类型

## 导航菜单

- [@select=""]()事件
  - 默认传入==el-menu-item==里的==index值==(字符串)、路径(数组)
  - select绑定的方法也可以普通调用，传入字符串索引，只要都是用v-show等控制显示
  - 无法用`func($event，$event，自定义变量)`，因为`$event`只能表示事件传入的==第一个==默认值

## 描述列表

- `<el-descriptions-item>`无法用`v-show`控制显示，必须用`v-if`

## 弹框

- 带输入框
  - 样例中的`{value}`是解构赋值，字段名不可变
- 自定义内容
  - 可以使用第三方组件样式，==但是==必须知道`el-button`标签里有哪些样式元素，有很大局限性。
  - $createElement方法第二个参数对象内==只能传入class、style==这些可识别字段，像icon这种自定义属性无法添加

## 输入框

- 添加自定义图标
  - 在`<el-input>`标签中，用`<i slot="suffix">`==插入==自定义图标

## Card卡片

- 因为`el-card__body`没有固定宽高，因此在卡片内使用==百分比==设置元素大小==行不通==，但是使用`px`单位的元素可以，并可以设置==overflow==

## 分页按钮

- [current-change]()：回调参数是==当前页==。==所有改变当前页==码的行为都会触发此方法