### Tips：

- 修改element样式必须把element样式文件引入时放前面
- element默认**横向铺满**父容器，所以要在使用element的地方用一个容器包裹起来限制宽度，**但是**element有默认高度(绝对大小)，并不会铺满整个父容器
- 二次封装就是“vue.component{}”中写入element ui等其他组件，并用**“prop”**属性从component外传入element ui元素内，“原生事件”还是写在组件内
- 目前element ui不支持Vue3
- element搭配自适应效果并不好，因为**内部**都有最小的**绝对尺寸**，比如padding之类的，扩大还行，但如果小到一定程度，就无法达到很小的效果了，像”input输入框“之类的**自动填充组件**可以被”**父容器**“压缩，但是“button按钮”之类**本身就不大的组件**，靠“父容器”**无法**进行**压缩**
- element组件的方法传入参数时，必须用`**:或者v-bind**`

------

给elementUI绑定自定义事件是不行的，必须要事件后加“**.native**”属性

------

使用自定义icon：必须用before > content占位，并将文字隐藏才能显示出来自定义图标

------

element中的label属性是标签对应的值，如果标签后无文字labels属性就充当文字

------

**table**的**列标签**可以加**“class-name”**属性添加样式

table标签下**添加样式**有**xxx-class-name:"className"**和**:xxx-class-name:"function({row,index}){if(index==n){return 'className}}"**两种形式

对于需要转换的标识符使用**“:formatter”**函数，其中默认传递的第一个参数**“row”**包含对象数据，第二个参数**“column”**包含element自己定义的一些属性方法(一般用不到)

插入单个元素用slot即可，但如果一个单元格内有多个元素标签呢？使用**“<template slot-scope='scope(固定名称)'”**，表示一个范围域；他可以获取到表格每一行数据，scope包含“row（每一行的数据）”、“column”、“$index（每一行索引值）”、“store（table 内部的状态管理）”

------

### 树形控件：

​    每个节点都是.el-tree-node，如果其下还有子节点则用.el-tree-node__children包裹.el-tree-node

​    节点前加样式的关键点在于.el-tree-node的after和before的**大小**以及**top**属性，构造一个横向线和竖向线（注意节点本身是并排的，所以要给节点和子节点都加padding-left使其错落开）

------

### form表单

​    在表单**头部**用“:rules”接收所有规则，在**表单项**用“prop”接收“:rules”中的“单个规则(数组)”，“单个规则”又是一个数组，其中由**多个对象**表示**多条规则**

​    使用:rule="rule"绑定验证规则时，如果是使用element提供的方法一次只能验证一个条件，例如

​        `{required: true, message: '年龄不能为空'}，

​         {type: 'number', message: '年龄必须为数字值'}`

​    多个条件就只能用多个对象表示；

​    但如果用自定义函数验证，则需要用关键词**'validator'**，

​        `{ **validator**: func, trigger: 'blur' }`

​        func:function(rule，**value**，**callback**){

​            if（**value**==条件）{```代码```}else{ **callback(new error('错误提示'))** }

​        }

​    form表单有个重要方法——“**validate**( valid=>{})”，它的返回值返回的是一个布尔值，如果为**true**则说明自定义验证规则都通过了，这个功能可以用于提交表单的时候，如果返回结果为true，则可以提交

#### el-form-item：

  会继承el-form的label-width；但**嵌套**在 el-form-item 中的 el-form-item 标签宽度默认为零，**不会继承** el-form 的 label-width；

------

### el-radio-group：

  包裹单选，就可以用label切换，在同时显示的情况下知道你点的是哪一个，radio靠label传值，如果没有设置标签内显示的文字，则默认显示label内容

![img](https://upload-images.jianshu.io/upload_images/6322775-77fd45a188be6b26.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

el-radio-button

------

### el-option：

  label是用于显示于页面的内容，value是选中后传递给后端的值，`el-select` 中的 `v-model` 绑定传递给后端的值；

  如果少了 `label` `el-select` 显示的值会变成 `v-model` 中的值

------

### 按钮：

  用·**<el-button/>**·是必定有按钮边框的，要想没有边框可以使用·**<i class="图片地址" />**·

  ·**<i class="图片地址" />**·大小和颜色由·**font-size**·和·**color**·决定，可以把它视作异形字体