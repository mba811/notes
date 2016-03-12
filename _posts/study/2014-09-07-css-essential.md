---
layout: post
title: CSS Essential
tags: [study]
---

> #### What is CSS? 
> 
> * * *
> 
> The basic goal of the Cascading Stylesheet (CSS) language is to allow a browser engine to paint elements of the page with specific features, like colors, positioning, or decorations. 
> 
> CSS主要作用是设定元素（通常称为盒子）的属性，并将设定这些元素在页面的位置关系。CSS解决的主要问题是： 
> 
>   1. 如何选择指定的元素？选择器（Selector）
>   2. 如何设定元素的属性？盒子模型（Box Model）
>   3. 如何设置元素间的位置关系？定位机制（Positioning Scheme）

本文主要介绍CSS的基本用法，包括如何使用选择器，如何设定样式以及如何布局定位，这些涉及到选择器、盒子模型、普通流、可视化格式模型和块格式化环境等核心概念，理解这些核心概念是熟练运用CSS的基础。 

若要详细了解相关内容，可参考文中链接，以及[MDN][10]、[W3][11]和[w3school][12]关于CSS的详细介绍。 

   [10]: https://developer.mozilla.org/en-US/docs/Web/CSS (Cascading Style Sheets)
   [11]: http://www.w3.org/Style/CSS/ (Cascading Style Sheets home page)
   [12]: http://www.w3school.com.cn/css/index.asp (CSS 教程)

## 基础知识 

### 基本语法 

CCS语法元素主要由选择器，属性和值构成，通过规则给选择器范围内的属性赋值。 

![CSS语法][13]

   [13]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_css-syntax-ruleset.png

CSS语法（规则） 

#### 各部分元素含义： 

  * `div p`：属性选择器
  * `#id`：id选择器
  * `first_line`：伪元素（pseudo-element）
  * `background-color`：属性（property）
  * `red`：值（value）

### 基本选择器 

选择器相当于作用域规则，选择属性设置起作用的范围。另一个相关概念是块格式化环境。 

#### 常用选择器 

* 
通用选择器，匹配任何元素。 

E 
标签选择器，匹配所有使用E标签的元素。 

.info 
class选择器，匹配所有class属性中包含info的元素。 

#footer 
id选择器，匹配所有id属性等于footer的元素。 

**id选择器和class选择器的区别：**

  * 在CSS文件中，id加前缀`#`，class用`.`；
  * 同一个页面id只可使用一次，class可多次引用：id是一个标签，用于区分不同的结构和内容，就象名字；class是一个样式，可套在任何结构和内容上，就象衣服，同一内容也可以套多个class的样式；
  * 从概念上说不一样：id是先找到结构/内容，再给它定义样式；class是先定义好一种样式，再套给多个结构/内容。
    
    <style>
    .content {font-size: 1.2em; line-height: 200%}
    .emphasis {font-weight: bold}
    #lily {color: white; background: black}
    </style>
    <div class="content emphasis" id="lily">The flower of lily of the valley is like tinkler, be born at spending cauline top to show raceme.</div>
    <div class="content" id="lavender">The air was fragrant with lavender.</div>

#### 多选择器组合 

E,F 
多元素选择器，同时匹配所有E元素或F元素，E和F之间用逗号分隔。 

E F 
后代元素选择器，匹配所有属于E元素后代的F元素，E和F之间用空格分隔。 

E > F 
子元素选择器，匹配所有E元素的子元素F。 

E + F 
毗邻元素选择器，匹配所有紧随E元素之后的同级元素F。 

如果两个选择器紧连，表示同时满足两个条件的内容。 
    
    <style>
    div.content {font-size: 1.2em; line-height: 200%}
    div.emphasis {font-weight: bold}
    div#lily {color: white; background: black}
    </style>
    <div class="content emphasis" id="lily">
    The flower of lily of the valley is like tinkler, be born at spending cauline top to show raceme.
    </div>

上例中，`div.emphasis`表示套用`.emphasis`样式的`div`标签。 

### 优先级别 

基本规则是`行内样式 > id样式 > class样式 > 标签名样式`，也就是，选择越具体优先级越高。如下元素： 
    
    <div id="ID" class="CLASS" style="color:black;"></div>

作用在其上样式的优先级从低到高是`div < .CLASS < div.CLASS < #ID < div#ID < #ID.CLASS < div#ID.CLASS`。 

**继承与覆盖：**

子元素没有设置的属性会从父元素继承而来。同一元素的同一属性如有多次设置，优先级高的覆盖优先级低的；若是同一优先级，后设设置覆盖前设置。 
    
    <style>
    div {font-size: 1em}
    div.content {font-size: 1.2em; line-height: 200%}
    div.emphasis {font-size: 1.4em; font-weight: bold}
    div#lily {color: white; background: black}
    </style>
    <div class="content emphasis" id="lily">
    The flower of lily of the valley is like tinkler, be born at spending cauline top to show raceme.
    <div id="water_lily"></div>
    </div>

上例中，`div`设定的`font-size`优先级太低，不会生效；`.emphasis`覆盖`.content`的`font-size`，`#lily`的`font-size`最终为`1.4em`；`#water_lily`继承了`#lily`的所有属性。 

### 属性赋值 

确定属性最终取值要[经历3步][14]（[CSS2经历4步][15]）：首先获取CSS样式中的指定值（specified value），然后如有必要则转换为绝对值或计算值（computed value），最后根据局部环境的约束再转换为实际值（actual value）。 

   [14]: http://www.w3.org/TR/2001/WD-css3-values-20010713/#specified (Specified, computed, and actual values)
   [15]: http://www.w3.org/TR/CSS2/cascade.html#value-stages (Specified, computed, and actual values)

指定值可能是绝对值（例如：`2mm`、`red`等），也可能是相对值（例如：`auto`、`1.2em`、`12%`等）。对于绝对值而言不需要计算即可获得计算值，相对值需要再借助参考值计算获得计算值。如果属性没有指定值，它的取值继承父元素。 

在特定的客户端环境，可能还需要将计算值转换为实际值，比如可能需要将小数的边界近似取整。 

> #### 常用的赋值规则 
> 
> * * *
> 
>   1. [颜色][16]赋值的三种形式：关键字、RGB空间、HSL空间；
>   2. [长度][17]赋值的两种形式：以`em`、`ex`、`ch`、`rem`、`vw`、`vh`、`vmin`、`vmax`为单位的相对赋值，以`cm`、`mm`、`in`、`pt`、`pc`、`px`为单位的绝对赋值；
>   3. [百分数][18]赋值：`width`、`margin`和`padding`等接受百分数赋值。
> 
>    [16]: https://developer.mozilla.org/en-US/docs/Web/CSS/color_value (<color>)
   [17]: https://developer.mozilla.org/en-US/docs/Web/CSS/length (<length>)
   [18]: https://developer.mozilla.org/en-US/docs/Web/CSS/percentage (<percentage>)

示例： 
> 
> `em`等单位和百分数都是相对赋值，需要继承父属性的参考值。`em`是针对字体大小的值，$w$ `em` = $w$ $\times$ `font-size`。 
>     
>     <div style="font-size:18px;">
>       Full size text (18px)
>       <span style="font-size:50%;">50%</span>
>       <span style="font-size:200%;">200%</span>
>     </div>
> 
> 上述代码中，`50%`从父属性继承而来的值是`18px`，然后再乘以`50%`（等价于`0.5em`），实际大小是`9px`（[查看效果][19]）。 

   [19]: https://developer.mozilla.org/en-US/docs/Web/CSS/percentage (<percentage>)

当一个属性可以有多个方向可设置值时，存在形如以下简写的赋值规则： 

位置赋值规则

![1][20]
`border-width: 1em`

   [20]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_border1.png

![2][21]
`border-width: 1em 2em`

   [21]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_border2.png

![3][22]
`border-width: 1em 2em 3em`

   [22]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_border3.png

![4][23]
`border-width: 1em 2em 3em 4em`

   [23]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_border4.png

## 盒子模型 

CSS将HTML的元素（可认为是标签）定义为适合CSS处理的矩形盒（rectangular box）。[盒子模型（box model）][24]描述了这些矩形盒的尺寸、属性（颜色、背景和边框等）和位置特性，浏览器根据盒子模型实现页面的渲染与显示。 

   [24]: https://developer.mozilla.org/en-US/docs/Web/CSS/box_model

按照HTML的元素是否新开一行可分为块（block-level）元素和内联（inline）元素，这一特性对设置元素的布局至关重要。 

> #### HTML的块（block-level）元素和内联（inline）元素 
> 
> * * *
> 
> [块元素][25]充满父元素的所有空间，每个块元素都会另起一个新行显示。块元素之内还可以包含块元素和行内元素。HTML定义的块元素包括`<div>, <span>, <p>`等。[内联元素][26]只占据标签包含内容的空间，不会另起新行显示。内联元素通常只包含内联元素。HTML定义的内联元素包括`<span>, <code>, <textarea>`等。 
> 
>    [25]: https://developer.mozilla.org/en-US/docs/Web/HTML/Block-level_elements
   [26]: https://developer.mozilla.org/en-US/docs/Web/HTML/Inline_elemente

![d][27]
> 
>    [27]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_css_box-intro.png

上例中，`<p>`是块元素，新起一行显示，并且撑满了行，`<em>`是内联元素，紧接上一元素显示。 

CSS的`display`属性可设定HTML元素生成盒子的类型是块还是内联，常用的属性值有`block, inline, inline-block, none`。将`display`属性设为`block`，可将内联元素转为块元素。盒子模型有四类边：margin edge，border edge，padding edge，content edge。 

![CSS语法][28]

   [28]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_box_model.gif

盒子模型的四类边 

`.box {with: ...; height: ...}`设置的是只是`Content`尺寸。相关边的的设置方法是： 
    
    .box {
        width: ...;
        height: ...;
        padding: ...;
        border: ...;
        margin: ...;
    }

内联元素设置属性`height`和`width`是没有用的，致使它变宽变大的原因是内部元素的宽高`+padding`。 

满足一定条件俩个盒子的外边届（margin）会叠加，使得估计这俩个盒子的位置关系变得复杂。 

> #### 外边叠加（margin collapsing） 
> 
> * * *
> 
> 上边距（top margin）和下边距（bottom margin）有时会叠加，叠加后两者最大值作为两者间的边距。发生外边叠加的3种情况（[图解][29]）1： 
> 
>    [29]: http://www.cnblogs.com/cuishengli/archive/2012/06/22/2558859.html#CSS%20%E5%A4%96%E8%BE%B9%E8%B7%9D%E5%90%88%E5%B9%B6

  1. 兄弟（Adjacent siblings）块：相邻块中，兄的下边界和弟的上边界会叠加；
>   2. 父与首尾孩子（Parent and first/last child）块：父块和首个孩子的margin-top之间不被任何东西分隔，则它们会叠加；父块和最后孩子的margin-bottom之间不被任何东西分隔，则它们会叠加；
>   3. 空块（Empty blocks）：不存在border、padding、inline content、height或min-height分隔块的margin-top和margin-bottom，上下边界会叠加。
> 
> 注意事项： 
> 
>   * 当与负边界叠加时，叠加后的边界是最大正边界和最小边界之和；
>   * 浮动的（floating）和绝对定位的（absolutely positioned）元素不参与外边叠加（这是因为创建了新的块格式化环境，而边界叠加只发生在同一块格式化环境）。

## 可视化格式模型 

> #### 可视化格式模型解决的主要问题 
> 
> * * *
> 
>   1. 如何生成盒子？
>   2. 盒子如何在页面布局？

[可视化格式模型（Visual formatting model）][30]是用于处理网页文档并显示到虚拟设备上的算法，该模型将文档中的每个元素生成符合盒子模型的盒子，然后对这些模型进行布局。可视化格式模型包括盒子的生成和定位两部分。 

   [30]: https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Visual_formatting_model

### 生成机制（Box Generation） 

生成盒子的类型通过CSS属性`display`设定，当设定属性为`block`、`list-item`和`table`时，生成块盒子，当设定属性为`inline`、`inline-block`和`inline-table`时，生成内联盒子。HTML元素的属性决定了该元素生成盒子类型的默认值，块元素默认生成块盒子，内联元素默认生成内联盒子。不同类型盒子的定位机制不同。 

### 定位机制（Positioning Scheme） 

CSS的盒子有3种基本定位机制：普通流（normal flow）、浮动（floats）和绝对定位（absolute position），默认定位机制是普通流。在普通流中，盒子一个接一个排列，floats算法可以将盒子从普通流中抽出来，绝对定位通过包含它盒子的坐标系统来定位。 

#### Normal Flow 

> #### 如何进入普通流（normal flow） 
> 
> * * *
> 
> CCS将盒子的`position`属性设置为`static`或`relative`，并且将`float`属性设置为`none`。默认值`position: static`，`float: none`。 
> 
> 普通流可以理解为盒子按照读入的先后次序依次处理，就像水流一样连续有序，每个页面对应一个流。浮动和绝对定位，可将读入序列中的某些盒子从这个流中抽取出来，单独处理。 

在普通流中，块盒子（block-level boxes ）和内联盒子（inline boxes）分别从纵向和横向对元素进行布局。块盒子通过垂直的方式一个接一个的排列，盒子之间的距离通过垂直方向的边界（margin-top和margin-bottom）控制（计算距离时要注意盒子外边叠加问题）。内联盒子通过水平方式排列，设置垂自方向的padding、borders和margins无效。水平排列的内联盒子通过行盒子（line box）组织在一起，行盒子的高度总是足够容纳包含在其中的所有盒子，可以通过设置行高（line height）控制行盒子的高度。因此，改变内联盒子尺寸的参数只有水平方向上的borders、padding、margins以及line height。 

![CSS语法][31]

   [31]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_css-syntax-ruleset_line_box.jpeg

包含在行盒子中的内联元素 

CSS2.1可以设置`display`属性为`inline-block`，融合inline和block的属性。在水平方向按内联盒子的方式布局，同时可以像块盒子一样设置widths、heights和垂自方向的 margins、padding。 

![设置relative的效果][32]

   [32]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_relative.png

设置relative的效果"position: relative; left: 20px; top: 20px;" 

#### Floats 

> #### 如何使用浮动模式（floats） 
> 
> * * *
> 
> CCS将盒子的`float`属性设置为`left`或`right`，并且将`position`属性设置为`static`或`relative`。 
> 
> `float`的非`none`属性暗示了盒子是`block`类型，因此，非块类型盒子的`display`属性会因为设置了`float`属性而改变为块类型。 

当盒子采用floats算法定位时，盒子会从普通流中被抽取出来，向左或向右移动，直到遇到父盒子或设置了float属性盒子的边界。 

![设置floats的效果][33]

   [33]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_floats.png

设置`float`的效果 

上图3个红色的方块，两个左浮（`float: left`），一个右浮。第二个左浮窗口位于第一个左浮动窗口右边，如果再增加左浮窗口，会不断的照此叠加，充满父盒子后会换行继续显示。 

一个盒子设置了浮动后，会影响它的兄弟元素，具体的影响方式较为复杂，这要视乎这些兄弟元素是块级元素还是内联元素，若是块级元素会无视这个浮动的块框，使自身尽可能与这个浮动元素处于同一行，导致被浮动元素覆盖，除非这些`div`设置了宽度，并且父元素的宽度不足以包含它们，这样兄弟元素才会被强制换行；若是内联元素，则会尽可能围绕浮动元素。 

浮动元素脱离了普通流，因此包含它的父元素不会因为这个浮动元素的存在而自动撑高，这就造成了高度塌陷。下图所示，由于左边盒子浮动而脱离了普通流，父元素的高度只是由`span`盒子决定，看起来就像父元素高度塌陷了。 

![浮动的影响][34]

   [34]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_clearfloat1.png

浮动的影响 

当浮动影响到盒子的布局时，需要清除浮动。 

#### Absolute Positioning 

> #### 如何采用绝对定位（absolute positioning） 
> 
> * * *
> 
> CCS将盒子的`position`属性设置为`absolute`或`fixed`。 
> 
> 当设置为`fixed`的时候，盒子的位置相对于浏览器可见的视窗固定，即使拖动浏览器的滚动条，盒子的位置也固定不变。 

![设置absolute的效果][35]

   [35]: http://jiyeqian.bitbucket.org/assets/images/2014-10-25-css-essential_absolute.png

设置absolute的效果"position: absolute; left: 20px; top: 20px;" 

#### 清除浮动（clearing floats） 

由于浮动元素会影响它的兄弟元素的位置和父元素产生高度塌陷，需要清除浮动。 

常用的清除浮动方法是`clear: both`，`clear`的属性值`both`、`left`、`right`、`none`、`inherit` 分别代表在元素左右两侧不允许出现浮动元素、左侧不允许出现浮动元素、右侧不允许出现浮动元素、不清除浮动、继承父元素的值。 

但是，`clear`只是清除了浮动对兄弟元素的影响，而高度塌陷问题还没有解决，需要更高级的清除浮动——闭合浮动。为什么叫闭合浮动？因为浮动的元素脱离了普通流，对于它的父元素，它并没有闭合，这时候就需要闭合浮动了。 

> #### 闭合浮动的3种方法 
> 
> * * *
> 
> （1）空`div`方法 
>     
>     <div class="box">
>         <div class="main left">我设置了左浮动 float: left</div>
>         <div style="clear: both;"></div>
>         <div class="aside">我是页脚，我的上面添加了一个设置了 clear: both 的空 div</div>
>     </div>
> 
> 空`div`方法很方便，但是加入了没有涵义的`div`，这违背了结构与表现分离的原则，并且后期维护也不方便。 
> 
> （2）`overflow`方法 
>     
>     <div class="box" style="overflow: hidden; *zoom: 1;">
>         <div class="main left">我设置了左浮动 float: left</div>
>         <div class="aside left">我是页脚，但是我也设置了左浮动。</div>
>     </div>
> 
> 当元素内包含会超出父元素边界的子元素时，`overflow`方法可能会覆盖掉有用的子元素，或是产生了多余的滚动条。 
> 
> （3）`:after`伪元素的方法 
>     
>     <style>
>         .clearfix {/* 触发 hasLayout */ zoom: 1; }
>         .clearfix:after {content: '.'; display: block; height: 0; clear: both; visibility: hidden; }
>     </style>
>     <div class="box clearfix">
>         <div class="main left">我设置了左浮动 float: left</div>
>         <div class="aside left">我是页脚，但是我也设置了左浮动。</div>
>     </div>
> 
> 这个办法不但完美兼容主流浏览器，并且也很方便，使用重用的类，可以减轻代码编写，另外网页的结构也会更加清晰。 

清除浮动的详细介绍可参考[《详说清除浮动》][36]（[示例][37]）。 

   [36]: http://kayosite.com/remove-floating-style-in-detail.html
   [37]: http://jiyeqian.bitbucket.org/assets/images/../html/clearfloat.html

## 块格式化环境 

> #### 块格式化环境（block formatting context） 
> 
> * * *
> 
> 块格式化环境是CCS渲染Web页面的一块区域，块盒子在该区域内布局。 
> 
> 块格式化环境对（`float`）定位和清除（`clear`）浮动至关重要，定位和清除浮动只对同一块格式化环境中的对象有效。`float`不会影响到其它块格式化环境中的盒子，`clear`只清除同一块格式化环境中之前的float效果。 
> 
> 简单来说，块格式化环境是一种属性，这种属性会影响着元素的定位以及与其兄弟元素之间的相互作用。 

块格式化环境就是一个作用范围，可理解为一个独立的容器，这个容器的里盒子的布局与这个容器外的不相干。 

块格式化环境不存在嵌套包含关系，块格式化环境只包含该环境内的对象，不会再包含该环境中对象再创建的块格式化环境。（A block formatting context contains everything inside of the element creating it that is not also inside a descendant element that creates a new block formatting context.） 

> #### 创建块格式化环境的条件（满足任意一条即可） 
> 
> * * *
> 
>   * the root element or something that contains it；
>   * `float`设置为**非**`none`；
>   * `position`设置为`absolute`或`fixed`；
>   * `display`设置为`inline-block`、`table-cell`、`table-caption`、`flex`或`inline-flex`；
>   * `overflow`设置为**非**`visible`。

块格式化环境主要有三个特性（详见[《详说 Block Formatting Contexts (块级格式化上下文)》][38]，[示例][39]）： 

   [38]: http://www.cnblogs.com/leejersey/p/3991400.html
   [39]: http://jiyeqian.bitbucket.org/assets/images/../html/bfc.html

  1. 阻止外边距折叠；
  2. 包含浮动的元素（清除浮动之`overflow`方法）；
  3. 阻止元素被浮动元素覆盖。

## 高级选择器 

#### CSS 2.1 属性选择器 

E[att] 
匹配所有具有att属性的E元素，不考虑它的值。（注意：E在此处可以省略，比如”[cheacked]”。以下同。） 

E[att=val] 
匹配所有att属性等于”val”的E元素。 

E[att~=val] 
匹配所有att属性具有多个空格分隔的值、其中一个值等于”val”的E元素。 

E[att|=val] 
匹配所有att属性具有多个连字号分隔（hyphen-separated）的值、其中一个值以”val”开头的E元素，主要用于lang属性，比如”en”、”en-us”、”en-gb”等等。 

示例： 
    
    p[title] { color:#f00; }
    div[class=error] { color:#f00; }
    td[headers~=col1] { color:#f00; }
    p[lang|=en] { color:#f00; }
    blockquote[class=quote][cite] { color:#f00; }

#### CSS 2.1 伪类（pseudo-classes） 

E:first-child 
匹配父元素的第一个子元素。 

E:link 
匹配所有未被点击的链接。 

E:visited 
匹配所有已被点击的链接。 

E:active 
匹配鼠标已经其上按下、还没有释放的E元素。 

E:hover 
匹配鼠标悬停其上的E元素。 

E:focus 
匹配获得当前焦点的E元素。 

E:lang(c) 
匹配lang属性等于c的E元素。 

示例： 
    
    p:first-child { font-style:italic; }
    input[type=text]:focus { color:#000; background:#ffe; }
    input[type=text]:focus:hover { background:#fff; }
    q:lang(sv) { quotes: "\201D" "\201D" "\2019" "\2019"; }

#### CSS 2.1 伪元素（pseudo-elements） 

E:first-line 
匹配E元素的第一行。 

E:first-letter 
匹配E元素的第一个字母。 

E:before 
在E元素之前插入生成的内容。 

E:after 
在E元素之后插入生成的内容。 

示例： 
    
    p:first-line { font-weight:bold; color;#600; }
    .preamble:first-letter { font-size:1.5em; font-weight:bold; }
    .cbb:before { content:""; display:block; height:17px; width:18px; background:url(top.png) no-repeat 0 0; margin:0 0 0 -18px; }
    a:link:after { content: " (" attr(href) ") "; }

更多关于CSS选选择器类容可参考[CSS reference][40]。 

   [40]: https://developer.mozilla.org/en-US/docs/Web/CSS/Reference

## 使用技巧 

> #### !important规则 
> 
> * * *
> 
> 多条CSS语句互相冲突时，具有!important的语句将覆盖其他语句。由于IE不支持!important，所以也可以利用它区分不同的浏览器。 
>     
>     h1 {color: red !important; color: blue;}

> #### 容器水平居中 
> 
> * * *
> 
> 先为该容器设置一个明确宽度，然后将margin的水平值设为auto即可。 
>     
>     div#container {width: 760px; margin: 0 auto;}

> #### 禁止自动换行 
> 
> * * *
> 
> 文字在一行中显示完成，不要自动换行。 
>     
>     h1 { white-space: nowrap; }

> #### 图片宽度自适应 
> 
> * * *
> 
> 如何使得较大的图片，能够自动适应小容器的宽度？ 
>     
>     img {max-width: 100%}

> #### 设置link状态的顺序 
> 
> * * *
> 
> link的四种状态，需要按照下面的前后顺序进行设置。 
>     
>     a:link 
>     a:visited 
>     a:hover 
>     a:active

> #### Text-transform和Font Variant 
> 
> * * *
> 
> Text-transform用于将所有字母变成小写字母、大写字母或首字母大写。 
>     
>     p {text-transform: uppercase} 
>     p {text-transform: lowercase} 
>     p {text-transform: capitalize}
> 
> Font Variant用于将字体变成小型的大写字母（即与小写字母等高的大写字母）。 
>     
>     p {font-variant: small-caps}

> #### 用图片充当列表标志 
> 
> * * *
> 
> 默认情况下，浏览器使用一个黑圆圈作为列表标志，可以用图片取代它。 
>     
>     ul {list-style: none}
>     ul li {
>         background-image: url("path-to-your-image");
>         background-repeat: none;
>         background-position: 0 0.5em; 
>     }

> #### 用图片替换文字 
> 
> * * *
> 
> 在标题栏中使用图片，但是又必须保证搜索引擎能够读到标题。 
>     
>     h1 { 
>         text-indent: -9999px; 
>         background: url("h1-image.jpg") no-repeat; 
>         width: 200px;
>         height: 50px;
>     }

## 预处理器 

CSS预处理器（css preprocessor）的基本思想是，用一种专门的编程语言，进行网页样式设计，然后再编译成CSS文件。常用的CSS预处理器有[Less][41]、[Sass][42]等。 

   [41]: http://lesscss.org
   [42]: http://sass-lang.com

### Less 

[Less][43]使用变量（variables）、混合（mixins）、函数（functions）和许多其他的技术，让你的CSS更具维护性、主题性、扩展性。Less可运行在Node环境，浏览器环境和Rhino环境，同时也有3种可选工具供你编译文件和监视任何改变。 

   [43]: http://lesscss.org

具体用法可参考Bootstrap中文网的[Less教程][44]。 

   [44]: http://www.bootcss.com/p/lesscss/ (LESS « 一种动态样式语言)

### Sass & Compass 

[Sass][45]使用变量（variables）、混合（mixins）、嵌套规则（nested rules）、内联导入（inline imports）等，让CSS更加优雅和强大。Sass让CSS更好的组织、体积更小、速度更快。 

   [45]: http://sass-lang.com

[Compass][46]是Sass的工具库（toolkit）。Sass本身只是一个编译器，Compass在它的基础上，封装了一系列有用的模块和模板，补充Sass的功能。它们之间的关系，有点像Javascript和jQuery、Ruby和Rails、python和Django的关系。 

   [46]: http://compass-style.org

具体使用可参考阮一峰的[Sass][47]和[Compass][48]用法指南。 

   [47]: http://www.ruanyifeng.com/blog/2012/06/sass.html (SASS用法指南)
   [48]: http://www.ruanyifeng.com/blog/2012/11/compass.html (Compass用法指南)

## 参考资料 

  1. [MDN: CSS][49]
  2. [W3: Cascading Style Sheets home page][50]
  3. [w3school: CSS 教程][51]
  4. [W3: Specified, computed, and actual values][52]
  5. [W3: CSS Values and Units Module Level 3][53]
  6. [W3: CSS Color Module Level 3][54]
  7. [W3: CSS basic box model][55]
  8. [W3: CSS Grid Layout Module Level 1][56]
  9. [W3: CSS Flexible Box Layout Module Level 1][57]
  10. [阮一峰：CSS使用技巧][58]
  11. [阮一峰：CSS选择器笔记][59]
  12. [cuishengli：CSS 框模型概述][60]
  13. [leejersey：详说 Block Formatting Contexts (块级格式化上下文)][61]
  14. [Kayo：详说清除浮动][62]
  15. [Jean: 8 CSS preprocessors to speed up development time][63]
  16. [LESS « 一种动态样式语言][64]
  17. [阮一峰：SASS用法指南][65]
  18. [阮一峰：Compass用法指南][66]

   [49]: https://developer.mozilla.org/en-US/docs/Web/CSS (Cascading Style Sheets)
   [50]: http://www.w3.org/Style/CSS/ (Cascading Style Sheets home page)
   [51]: http://www.w3school.com.cn/css/index.asp (CSS 教程)
   [52]: http://www.w3.org/TR/2001/WD-css3-values-20010713/#specified (Specified, computed, and actual values)
   [53]: http://www.w3.org/TR/css3-values/ (CSS Values and Units Module Level 3)
   [54]: http://www.w3.org/TR/2011/REC-css3-color-20110607/
   [55]: http://dev.w3.org/csswg/css-box/
   [56]: http://dev.w3.org/csswg/css-grid/
   [57]: http://dev.w3.org/csswg/css-flexbox/
   [58]: http://www.ruanyifeng.com/blog/2010/03/css_cookbook.html
   [59]: http://www.ruanyifeng.com/blog/2009/03/css_selectors.html
   [60]: http://www.cnblogs.com/cuishengli/archive/2012/06/22/2558859.html
   [61]: http://www.cnblogs.com/leejersey/p/3991400.html
   [62]: http://kayosite.com/remove-floating-style-in-detail.html
   [63]: http://www.catswhocode.com/blog/8-css-preprocessors-to-speed-up-development-time (8 CSS preprocessors to speed up development time)
   [64]: http://www.bootcss.com/p/lesscss/ (LESS « 一种动态样式语言)
   [65]: http://www.ruanyifeng.com/blog/2012/06/sass.html (SASS用法指南)
   [66]: http://www.ruanyifeng.com/blog/2012/11/compass.html (Compass用法指南)

### 脚注 

  1. 也可参考”CSS Mastery: Advanced Web Standards Solutions, Second Edition”一书的”Chapter 3: Visual Formatting Model Overview”。 ↩

