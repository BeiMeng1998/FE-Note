# CSS语法

CSS语法：
        选择器 {声明块}
        选择器：
        通过选择器可以选中页面中指定的元素
        并且将声明块中的样式应用到选择器对应的元素上
        声明块：
        声明块紧跟在选择器后面，使用 {} 括起来
        声明块中实际上就是一组一组的名值对结构
        这一组一组的名值对我们称为声明
        在一个声明块中可以写多个声明，多个声明之间使用 ; 隔开
        声明的样式名和样式值之间使用 : 来连接

# CSS选择器优先级

当使用不同的选择器，选中同一个元素时并且设置相同的样式时
这时样式之间产生了冲突，最终到底采用哪个选择器定义的样式，由选择器的优先级决定，优先级高的优先显示

优先级的规则：
内联样式    优先级 1000
ID选择器    优先级 0100
类，伪类，属性    优先级 0010
元素选择器 优先级 0001
通配选择器 优先级 0000
继承的样式 没有优先级

当选择器中包含多种选择器时，需要将多种选择器的优先级不进位相加然后再比较
但是注意，选择器优先级的计算不会超过它的最大数量级，如果选择器的优先级一样，则使用新声明的样式

并集选择器的优先级是单独计算
例如：`div , p , #p1 , .hello {}`

可以在样式的最后，添加一个 !important ,则此时该样式会获得一个最高优先级（超过内联样式）尽量避免使用

# CSS选择器

## 通配选择器

通过通配选择器选中页面中所有的元素
语法：* {}

例：为页面所有元素设置颜色为红色
```
* {
    color: red;
}
```

## 元素选择器

通过元素选择器可以选择页面中所有指定元素
语法：标签名 {}

例：为页面中所有的P元素，设置字体颜色为红色
```
p {
    color: red;
}
```

## id选择器

通过元素的id属性值选中唯一的一个元素
语法：#id {}

例：为id属性值为p1的标签设置字体大小为20px
```
#p1 {
    font-size: 20px;
}
```

## 类选择器

通过元素的clss属性值选中一组元素
语法：.class {}

例：为clss属性值为hello的标签设置字体大小为50px
```
.hello {
    font-size: 50px;
}
```

## 并集选择器

通过并集选择器可以同时选中多个选择器对应的元素
语法：选择器1 ,  选择器2 ,  选择器3 {}
 , 表示结合符
例：为id为p1，class为p2的元素，以及h1设置背景颜色为黄色
```
#p1, .p2, h1 {
    background-color: yellow;
}
```

## 交集选择器/复合选择器

通过交集选择器选择同时满足多个选择器的元素
语法：选择器1选择器2选择器3 {}

例：为class为p3的span标签设置背景颜色为yellow
```
span.p3 {
    background-color: red;
}
```

## 子元素选择器

通过子元素选择器可以选择指定父元素的指定子元素
语法：父元素 > 子元素 {}

例：为div的子元素span设置背景颜色为黄色
```
div > span {
    background-color: yellow;
}
```

## 后代元素选择器

通过后代选择器可以选择指定元素的指定后代元素
语法：祖先元素 后代元素 {}

例：为div标签中的p标签内的span标签设置颜色为绿色
```
div p span {
    color: green;
}
```

## 兄弟元素选择器

通过兄弟元素选择器可以选择一个元素后紧挨着的指定的兄弟元素
语法：
元素1 + 元素2 {} 必须相邻
元素1 ~ 后面所有元素 {} 不一定要相邻

例：为span后一个p标签设置背景颜色为黄色
```
span + p {
    background-color: yellow;
}
```

例：为span后所有p标签设置背景颜色为黄色
```
span ~ p {
    background-color: yellow;
}
```

## 属性选择器

通过属性选择器可以根据元素中的属性或属性值来选取指定元素
语法：
[属性名] {} 选取含有指定属性的元素
[属性名="属性值"] {} 选取含有指定属性值的元素
[属性名^="属性值"] {} 选取属性值以指定内容开头的元素
[属性名$="属性值"] {} 选取属性值以指定内容结尾的元素
[属性名*="属性值"] {} 选取属性值包含指定内容的元素

例：为具有title属性的p元素，设置背景颜色为黄色
```
p[title] {
    background-color: yellow;
}
```

例：为title属性值为hello的元素设置背景颜色为黄色
```
[title="hello"] {
    background-color: yellow;
}
```

例：为title属性值以ab开头的元素设置背景颜色为黄色
```
[title^="ab"] {
    background-color: yellow;
}
```

例：为title属性值以c结尾的元素设置背景颜色为黄色
```
[title$="c"] {
    background-color: yellow;
}
```

例：为title属性值含有a的元素设置背景颜色为黄色
```
[title*="a"] {
    background-color: yellow;
}
```

## 伪元素选择器

使用伪元素来表示DOM树外的一些特殊位置
语法：
元素:first-line {} 选中元素第一行
元素:first-letter {} 选中元素第一个字符，只能用于块元素

元素:before/after {
    content: "";
} 选中元素最前面/最后面部分，一般配合content样式一起使用

例：为p第一行设置背景为黄色
```
p:first-line {
    background-color: yellow;
}
```

例：为p第一个字符设置颜色为黄色
```
p:first-letter {
    color: yellow;
}
```

例：为class属性值为a的元素的最前和最后添加一些内容
```
.a:before {
    content: "balabala";
}
.a:after {
    content: "balabala";
}
```

## 伪类选择器

使用伪类选择器来选择元素的某种特殊的状态

### 链接伪类

注意 :link :visited :target是作用于链接元素的
a:link {} 为没有访问过的链接设置样式

a:visited {} 为访问过的链接设置样式，浏览器是通过历史记录来判断一个链接是否访问过，由于涉及到隐私问题，所以使用visited伪类只能设置字体的颜色color background-color border-color

a:target 代表一个特殊元素，用于选取当前活动的目标元素

### 动态伪类

注意:hover :active基本可以作用于所有的元素
元素:hover {} 为鼠标移入状态设置样式
元素:active {} 为被点击的状态设置样式

注意：:hover 要在 :link :visited 之后，:active 要在 :hover 之后， :hover 和 :active 也可以为a以外的元素设置除IE6

L(link)OV(visited)E, H(hover)A(active)TE 爱与恨

例：为超链接设置样式
未访问时颜色为绿色
访问过的颜色为红色
鼠标移入状态下为黄色
被点击状态下为黑色
```
a:link {
    color: greem;
}
a:visited {
    color: red;
}
a:hover {
    color: yellow;
}
a:active {
    color: black;
}
```

### 表单相关伪类

元素:focus {} 为表单获取焦点后设置样式
元素:enabled 匹配可编辑的表单
元素:disabled 匹配被禁用的表单
元素:checked 匹配被选中的表单

例：文本输入框获取焦点后，修改背景颜色为黄色
```
input:focus {
    background-color: yellow;
}
```

### 结构性伪类

元素:selection {} 为元素选中内容设置样式，火狐用:-moz-selection
元素:nth-child (An+B){} 为任意位置的子元素设置样式，even表示偶数位置的子元素，odd表示奇数位的子元素,An+B:从B开始，每隔A选中，IE6~8不支持
元素:nth-last-child(){}
元素:first/last-child {} 为是第一个/最后一个子元素的该元素设置样式,IE6不支持
元素:nth-of-type {} child是在所有的子元素中排列，而type是在当前类型的子元素中排列，IE6~8不支持
元素:nth-last-of-type {}
元素:first/last-of-type {} child是在所有的子元素中排列，而type是在当前类型的子元素中排列


例：为是第一个子元素的p设置字体大小40px，且所有p选中状态下背景色为橙色
```
p:selection {
    background-color: orange;
}
p:first-child {
    font-size: 40px;
}
```

例：为第一个/最后一个p元素设置字体40大小px
```
p:first/last-of-type {
    font-size: 40px;
}
```

例：为第二个p元素设置字体40大小px
```
p:nth-of-type(2) {
    font-size: 40px;
}
```

## 否定伪类选择器

通过否定伪类选择器可以从已选中的元素中剔除某些元素
语法：
元素1:not(元素2) {}
元素:empty{}

例：为所有的p元素设置背景颜色为黄色，除了class属性值为hello的元素
```
p:not(.hello) {
    background-color: yellow;
}
```

# 文字样式

```
p {
    /*color设置文字颜色*/
    
    color: red;

    /*font-size设置字体大小，默认文字大小为16px
    
    font-size: 40px;

    /*font-family设置文字字体
     在网页中将字体分成5大类：
	 serif（衬线字体）
     sans-serif（非衬线字体）
     monospace （等宽字体）
     cursive （草书字体）
     fantasy （虚幻字体）
     可以将字体设置为这些大的分类,当设置为大的分类以后，
     浏览器会自动选择指定的字体并应用样式
     一般会将字体的大分类，指定为font-family中的最后一个字体
    */
    
    font-family: serif;

    /*font-style设置文字斜体
     可选值：
        normal 默认值，文字正常显示
        italic 文字会以斜体显示
        oblique 文字会以倾斜的效果显示
        italic和oblique效果往往一样
        一般都用italic
    */
    
    font-style: italic;

    /*font-weight设置文字加粗效果
     可选值：
         normal 默认值，文字正常显示
         bold 文字加粗显示
     该样式也可以指定100-900之间的9个值，但是由于用户的计算机往往没有这么多级别的字体
    */
    
    font-weight: bold;

    /*font-variant设置小型大写字母
     可选值：
         normal 默认值，文字正常显示
         small-caps 文本以小型大写字母显示
    */

    font-variant: small-caps;

    /*font样式简写文字样式
     文字的大小必须是倒数第二个样式
     文字的字体必须是倒数第一个样式
    */

    font: small-caps bold italic 60px "微软雅黑";

}
```

# 文本样式

```
P {
    /*设置行间距的方式
     CSS中没有提供直接行间距的方式
     只能通过设置行高来间接地设置行间距，行高越大行间距越大
     line-height设置行高
     行间距 = 行高 - 字体大小
     可选值：
         1.指定一个大小 如40px
         2.指定一个百分数，则会相对于文字大小去计算行高
         3.指定一个数值，则行高会设置文字大小的相应的倍数

     font样式也可以指定行高在字体大小后加 /可选值
    */

    line-height: 2;
    font: 30px/2 sans-serif;

    /*text-transform设置文本大小写
     可选值：
         none 默认值 不做处理
         capitalize 单词首字母大写 通过空格识别单词
         uppercase 所有字母都大写
         lowercase 所有字母都小写
    */

    text-transform: lowercase;

    /*text-decoration设置文本的修饰
     可选值：
         none 默认值 不加任何修饰
         underline 为文本加下划线
         overline 为文本加上划线
         line-through 为文本加删除线
     
     超链接会默认添加下划线，需要将文本装饰样式设置为none
    */

    text-decoration: line-through;

    /*letter-spacing设置字符间距*/

    letter-spacing: 10px;

    /*word-spacing设置单词间距*/

    word-spacing: 20px;

    /*text-align设置文本对齐方式
     可选值：
         left 默认值，文本靠左对齐
         right 文本靠右对齐
         center 文本居中对齐
         justify  两端对齐 原理：通过调整文本之间的空格大小，来达到两端对齐
    */

    text-align: justify;

    /*text-indent设置文本首行缩进
     正值 向右缩进
     负值 向左缩进
     一般em作单位
    */

    text-indent: 2em;

    /*white-space设置文本是否换行
     可选值：
         normal 连续的空白符会被合并，换行符会被当作空白符来处理。填充line盒子时，必要的话会换行。
         nowrap 和 normal 一样，连续的空白符会被合并。但文本内的换行无效。
         pre 连续的空白符会被保留。在遇到换行符或者<br>元素时才会换行。
         pre-wrap 连续的空白符会被保留。在遇到换行符或者<br>元素，或者需要为了填充line盒子时才会换行。
         pre-line 连续的空白符会被合并。在遇到换行符或者<br>元素，或者需要为了填充line盒子时会换行。
         break-spaces 与 pre-wrap的行为相同，除了：
             任何保留的空白序列总是占用空间，包括在行尾。
             每个保留的空格字符后都存在换行机会，包括空格字符之间。
             这样保留的空间占用空间而不会挂起，从而影响盒子的固有尺寸（最小内容大小和最大内容大小）。
    */

    white-space:nowarp;

    /*text-overflow属性确定如何向用户发出未显示的溢出内容信号。它可以被剪切，
    显示一个省略号或显示一个自定义字符串。
     可选值：
        clip
            这个关键字的意思是"在内容区域的极限处截断文本"，因此在字符的中间可能会发生截断。
            为了能在两个字符过渡处截断，你必须使用一个空字符串值 ('')。此为默认值。
        ellipsis
            这个关键字的意思是“用一个省略号 ('…', U+2026 HORIZONTAL ELLIPSIS)来表示被截断的文本”。
            这个省略号被添加在内容区域中，因此会减少显示的文本。如果空间太小到连省略号都容纳不下，
            那么这个省略号也会被截断。
        <string> 
            <string>用来表示被截断的文本。字符串内容将被添加在内容区域中，所以会减少显示出的文本。
            如果空间太小到连省略号都容纳不下，那么这个字符串也会被截断。
}
```

# 块元素盒子模型样式

```
div {
    /*width设置盒子内容区宽度
     height设置盒子内容区高度
     width和height只设置盒子内容区的大小，而不是盒子的整个大小
     盒子可见框的大小由内容区，内边距和边框共同决定
    */

    width: 40px;
    height: 40px;

    /*background-color设置背景颜色*/

    background-color: red;

    /*border-width设置边框宽度
     使用border-width可以分别指定四个边框宽度
     如指定四个值，则四个值会分别设置给 上 右 下 左
     如指定三个值，则三个值会分别设置给 上 左右 下
     如指定两个值，则两个值会分别设置给 上下 左右
     如指定一个值，则这个值会设置给 上下左右
     除了border-width，CSS中还提供了
     border-top-width
     border-right-width
     border-bottom-width
     border-left--width
    */

    border-width: 10px;

    /*border-color设置边框颜色
     用法与border-width相同
    */

    border-color: red;

    /*border-style设置边框样式
     用法与border-width相同
     可选值：
         none 默认值 没有边框
         solid 实线
         dotted 点状边框
         dashed 虚线
         double 双线
    */

    border-style: solid dotted dashed double;

    /*border简写设置边框宽度，颜色，样式
     并且没有任何的顺序要求
     但border一指定就是四边同时设置，不能分别设置
     可以使用
     border-top
     border-right
     border-bottom
     border-left
     分别设置各边
    */

    border: red solid 10px;

    /*padding设置内边距（盒子的内容区与盒子边框之间的距离）
     使用方法与border-width相同
    */

    padding: 10px 20px 30px 40px;

    /*margin设置盒子外边距（盒子与盒子之间的距离）
     使用方法与border-width相同
     margin还可以设置为auto，auto一般只设置给水平方向的margin（水平居中）
     垂直外边距的重叠：
         在网页中相邻的垂直方向的外边距会发生外边距重叠
         兄弟元素之间的相邻外边距会取最大值而不是取和
         父子元素 垂直 外边距相邻了，则子元素的外边距会设置给父元素
    */

    margin: 0 auto;

    /*浏览器有默认的margin和padding
     使用通配选择器将默认样式清除
    */

    /*overflow设置父元素如何处理溢出内容
     子元素默认是存在于父元素的内容区
     如果子元素的大小超过了父元素的内容区
     则超过的部分会溢出父元素，在父元素以外显示
     可选值：
         visible 默认值 不对溢出内容处理
         hidden 将溢出的内容隐藏不显示
         scroll 会父元素添加滚动条，不论内容是否溢出，都会添加水平和垂直双方向的滚动条
         auto 会自动根据需求添加滚动条
    */

    overflow: auto;

    /*float设置元素浮动
     可选值：
         none 默认值 元素默认在文档流中排列
         left 元素脱离文档流 向页面左侧浮动
         right 元素脱离文档流 向页面右侧浮动
    
     当为一个元素设置浮动后，元素会立即脱离文档流向上浮动，直到遇到父元素边框或者其他浮动元素
     如果浮动元素上方是一个没有浮动的块元素，则浮动元素不会超过块元素
     浮动元素不会超过它上方的兄弟元素，最多一样高
     浮动元素不会盖住文字，文字会环绕图片
     在文档流中，子元素的宽度 默认 占父元素的全部
     当元素设置浮动脱离文档流后
     高度和宽度都被内容撑开
    */

    float: left;

    /*clear清除其他浮动元素对当前元素的影响
     可选值：
         none 默认值，不清楚浮动
         left 清除左侧浮动元素对当前元素的影响
         right 清除右侧浮动元素对当前元素的影响
         both 清除两侧侧浮动元素对当前元素的影响
    */

    clear:both;
}
```

# 内联元素盒子模型样式

```
p {
    /*内联不可替换元素不能设置width和height*/
    /*内联不可替换元素可以设置内边距，但垂直方向内边距不会影响页面布局*/
    /*内联不可替换元素可以设置边框，但垂直方向边框不会影响页面布局*/
    /*内联不可替换元素只可以设置水平方向外边距，不支持垂直方向外边距，而且水平方向的相邻元素外边距不会重叠，而是求合*/

    /*display修改元素类型
     可选值：
     inline 将元素类型修改为内联元素
     block 将元素类型修改为块元素
     inlin-block 将元素类型修改为行内块元素（既有块元素的特点，可以设置宽高，又不会独占一行）
     none 不显示元素，并且元素不会在页面中继续占有位置
    */

    display: block;

    /*visibility设置元素的隐藏和显示状态
     可选值：
         visible 默认值 元素显示
         hidden 元素隐藏不显示 但它的位置会依然保持
    */

    visibility: hidden;

    /*内联元素使用float浮动脱离文档流后会变成块元素*/

    float: left;
}
```

# 表格样式

```
table {
    width: 300px;
    margin: 0 auto;
    border: 1px solid black;
    /*border-spacing属性设置table和tr边框距离*/

    border-spacing: 0px;

    /*border-collapse设置表格的边框合并
     如果设置了边框合并，则border-spacing自动失效
    */

    border-collapse: collapse;
    background-color: red;
}
```

# 元素定位

通过position属性来设置元素的定位
    可选值：
        static 默认值 元素没有开启定位
        relative 元素开启相对定位
        absolute 开启元素的绝对定位
        fixed 开启元素的固定的定位（绝对定位的一种）

## 包含块

包含块简单说就是定位参考框或者定位参考坐标参考系，元素一旦定义了定位显示（相对，绝对，固定）都具有包含块性质，它所包含的定位元素都将以该包含块为坐标系进定位和调整

包含块是视觉格式化模型的一个重要概念，它与框模型类似，也可以理解为一个矩形，而这个矩形的作用是为它里面包含的元素提供一个参考，元素的尺寸和位置的计算往往是由该元素所在的包含块决定的

原理：一个元素盒子的位置和大小有时是通过相对于一个特定的长方形来计算的，这个长方形被称之为元素的containing box，一个元素的containing box按以下方式定义
    1.初始包含块是视口大小的矩形，不是视口
    2.对于其它元素，除非元素使用的绝对位置，包含块由最近的块级祖先元素盒子的内容边界组成
    3.如果元素有属性position:fixed包含块由视口建立
    4.如果元素有属性position:absolute包含块由最近的position不是static的祖先建立
        祖先为块元素，由祖先的padding edge组成
        如果没有祖先，包含块为初始包含块

## 相对定位

```
div {
    /*当元素的position属性设置为relative时，开启元素的相对定位
     1.不设置偏移量，元素位置不会发生任何变化
     2.相对定位是相对于元素在文档流原位置进行定位（left: 100px; 距离参照物左边100px）
     3.相对定位不会脱离文档流
     4.相对定位会使元素提升一个层级
     5.相对定位不会改变元素性质，块元素内联元素性质不会改变
     6.通过left right top bottom设置元素距离参照物的距离
    */
    
    position: relative;
    left: 100px;
    top: 100px;
}
```

## 绝对定位

```
div {
    /*当position属性设置为absolute时，开启元素的绝对定位
     1.不设置偏移量，元素位置不会发生任何变化
     2.绝对定位是相对于离它最近的开启了定位的祖先元素进行定位，
     如果所有的祖先元素都没有开启定位，则会相对于初始包含块进行定位
     3.绝对定位会脱离文档流
     4.绝对定位会使元素提升一个层级
     5.绝对定位会改变元素的性质
         内联元素变为块元素
         块元素的宽度和高度默认被内容撑开
    */

    position: absolute;
    left: 100px;
    top: 100px;
}
```

## 固定定位

```
div {
    /*当position属性设置为fixed时，开启元素的固定定位
     固定定位也是一种绝对定位，它的大部分特点都和绝对定位一样
     不同的是：
         固定定位永远都会相对于浏览器窗口进行定位
         固定定位会固定在浏览器窗口的某个位置，不会随滚动条滚动
    注：IE6不支持固定定位
}
```

# 元素层级与透明

```
div {
    /*z-index属性设置元素的层级
     可以为z-index指定一个正整数作为值，该值会作为当前元素的层级
     层级越高，越优先显示

     注：对于没有开启定位的元素不能使用z-index,父元素的层级再高也不会盖住子元素
    */

    z-index: 25;

    /*opacity属性设置元素背景的透明度
     可选值：
         0~1之间的值
         0 表示完全透明
         0.5 表示半透明
         1 表示不透明
     opacity属性在IE8及以下的浏览器中不支持
     
     IE8及以下浏览器用
     filter: alpha(opacity=透明度)
     透明度可选值：
         0~100之间的值
         0 表示完全透明
         50 表示半透明
         100 表示不透明

     注：此方法支持IE6，但IE Tester中无法测试
    */

    opacity: 0.5;
    filter: alpha(opacity=50);
}
```

# 元素背景

```
div {
    /*background-image设置背景图片
     语法：
         background-image: url(相对地址);
     如果背景图片大于元素，默认会显示图片的左上角
     如果背景图片和元素一样大，背景图片全部显示
     如果图片小于元素大小，则会默认将图片平铺以充满元素
     可以同时为一个元素指定背景颜色(background-color)和背景图片，
     这样背景颜色将会作为背景图片的底色，一般情况下设置背景图片时都会同时指定一个背景颜色
    */

    background-image: url(img/jpg1.jpg);
    background-color: red;

    /*background-repeat设置背景图片的重复方式
     可选值：
         repeat 默认值，背景图片双向重复（平铺）
         no-repeat 背景图片不会重复
         repeat-x 背景图片沿水平方向重复
         repeat-y 背景图片沿垂直方向重复
    */

    background-repeat: no-repeat;

    /*background-position设置背景图片在元素中的位置
     可选值：
         top right left bottom center中的两个值来指定一个背景图片的位置
         top left 左上
         bottom right 右下
         如果只给出一个值，则第二个值默认是center
         
         也可以直接指定两个偏移量
         第一个值是水平偏移量
             如果指定的是一个正值，则图片会向右移动指定的像素
             如果指定的是一个负值，则图片会向左移动指定的像素
         第二个是垂直偏移量
             如果指定的是一个正值，则图片会向下移动指定的像素
             如果指定的是一个负值，则图片会向上移动指定的像素
    */

    background-position: 10px 10px;

    /*background-attachment设置背景图片是否随页面一起滚动
     可选值：
         scroll 默认值，背景图片随着窗口滚动
         fixed 背景会固定在某一位置，不随页面滚动
     不随窗口滚动的图片，我们一般都是设置给body,而不设置给其他元素
    */

    background-attachment: fixed;

    /*background属性可以设置所有背景相关的样式
     没有顺序的要求，没有数量要求
     不写的样式都使用默认值
     background: red url(img/1.png) center center no-repeat fixed;
}
```