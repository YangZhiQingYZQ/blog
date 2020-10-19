# CSS盒子模型

## 标准模型的基本概念

标准模型【默认：box-sizing：content-box】由四部分组成，从内到外依次是：

1. 内容区域【content】：可以放置元素的区域如文本，图像等，一般设置宽高指的是这个内容的宽高
2. 内边距区域【padding】：内容与边框之间的距离
3. 边框区域【border】：就是边框
4. 外边框区域【margin】：由外边边距限制，用空白区域扩展边框区域，来分开相邻元素

## 标准模型与IE模型的区别

标准模型指的是设置box-sizing为content-box的盒子模型，一般width，height指的是content的宽高。而IE模型指的是box-sizing为border-box的盒子模型。宽高计算时content+padding+border

## 边距重叠

* 相邻元素之间
* 父元素与其第一个或最后一个子元素自检
* 空的块级元素

## BFC

### 定义
块格式化上下文（Block Formatting Context，BFC）是Web页面的可视CSS渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域；

块格式化上下文包含创建它的元素内部的所有内容

### 创建BFC的场景

* 根元素<html>
*  浮动元素（元素的float不是none）
*  绝对定位元素（元素的position为absolute或fixed）
*  行内元素（元素的display为inline-block）
*  表格单元格（元素的display为table-cell，HTML表格单元格默认为该值）
*  表格标题（元素的display为table-cation，HTML表格标题默认为该值）
*  匿名表格单元格元素（元素的display为table、table-row、table-row-group、table-header-group、table-footer-group（分别是HTMLtable、row、tbody、thead、tfoot的默认属性）或inline-table）
*  overflow值不为visible的块元素
*  display值为flow-root的元素
*  contain值为layout、content或paint的元素
*  弹性元素（display为flex或inline-flex元素的直接子元素）
*  多列容器（元素的column-count或column-white不为auto，包括column-count为1）
*  column-span为all的元素始终会创建一个新的BFC，即使该元素没有报过在一个多列元素中

### 作用

* 让浮动内容与周围内容等高，当浮动内容溢出容器的时候，创建容器的BFC，来包裹这个浮动
* 外边距崩塌：当外边距被合并时，创建各自的BFC，来解决父元素与第一个元素上边距合并，创建父元素与第一个元素的各自的BFC

