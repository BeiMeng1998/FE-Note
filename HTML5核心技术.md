# attribute与property

浏览器只认property，用户操作的是property

attribute：html标签的预定义和自定义属性，设置的属性值只能为字符串属性
property：JS原生对象的直接属性

attribute与property的同步关系

1.property为非布尔值属性时，二者实时同步

2.property为布尔值属性时，attribute只会在第一次设置时影响property，之后二者不再同步

所以操作布尔值属性最好用property，非布尔值属性attribute

attr()函数操作的是文档节点的属性，因此设置的属性值只能是字符串类型，如果不是字符串类型，也会调用其toString()方法，将其转为字符串类型。
prop()函数操作的是JS对象的属性，因此设置的属性值可以为包括数组和对象在内的任意类型。

# classList

classList是一个只读属性，返回一个元素的类属性的实时DOMTokenList伪数组对象
使用classList是替代className作为空格分隔的字符串访问元素的类列表的一种方便方法

## classList.add(string)
添加指定的类值，若这些类已经存在于元素的属性中，那么它们将被忽略

## classList.remove(string)
删除指定的类值

## classList.item(number)
按集合中的索引返回类值

## classList.toggle(string，force)
当只有一个参数时：切换class value;即如果类存在，则删除它并返回false,如果不存在，则添加它并返回true
当存在第二个参数时：如果第二个参数的计算结果为true;则添加指定的类值，如果计算结果为false,则删除它

## classList.contains(string)
检查元素的类属性中是否存在指定的类值

## classList.replace(oldclass, newclass)
用一个新类替换旧类

# HTML自定义属性

data-* 全局属性 是一类被称为自定义数据属性的属性，它赋予我们在所有HTML元素上嵌入自定义数据属性的能力，并可以通过脚本（一般指JavaScript）与HTML之间进行专有数据的交换
例如：
    `<div id="outer" data-name-a="蔡徐坤"></div>`

所有这些自定义数据属性都可以通过所属元素的HTMLElement接口来访问。HTMLElement.dataset属性可以访问它们
例如：`outer.dataset.nameA = "吴亦凡"`

*可以使用遵循xml名称产生规则的任何名称来被替换，并具有以下限制：
    1.该名称不能以xml开头，无论这些字母是大写还是小写
    2.该名称不能包含任何分号(U+003A)
    3.该名称不能包含任何大写字母

注意HTMLElement.dataset属性是一个DOMStringMap，并且自定义数据属性data-test-value可以通过HTMLElement.dataset.testValue(或者HTMLElement.dataset["testValue"])来访问，任何破折号（U+002D）都会被下个字母的大写替代（驼峰拼写）

# contenteditable属性

全局属性contenteditable是一个枚举属性，表示元素是否可被用户编辑。如果可以，浏览器会修改元素的部件以允许编辑
例如：
```
<blockquote contenteditable="true">
    <p>Edit this content to add your own quote</p>
</blockquote>
```
该属性必须是下面的值之一：
    true或空字符串：表示元素是可被编辑的
    false：表示元素不是可编辑的

# H5与H4的区别
 
编码 渲染模式 mine类型
①用于绘画的 canvas 元素；
②用于媒介回放的 video 和 audio 元素；
③对本地离线存储的更好的支持；
④新的特殊内容元素，比如 article、footer、header、nav、section；
⑤新的表单控件，比如 calendar、date、time、email、url、search。

```
HTML5余HTML4之间10个关键的不同之处
1.HTML5仍然是一个制定中的标准
这第一个也是非常重要的一个区别，虽然与HTML4相比HTML5很酷，很规范，但是这些都无法改变HTML5依然是一个制定中的标准的事实。HTML5仍然处在初级阶段，预期会发生很多变化。你必须把这些因素考虑进来，因为这个你需要不停的更新升级你的网站，这是很不方便的。这也是为什么到目前为止，最好在产品里使用HTML4，只在实验里使用HTML5的原因。HTML4也许已经超过10岁了，但是它作为正式标准的事实一直没变。

2.简化的语法
更简单的doctype声明是HTML5里众多新特征之一。现在只需要写就好了。HTML5的语法兼容HTML4和XHTML1，但不兼容SGML。

3.新的<canvas>标记代替flash
Flash给Web开发者带来了很多麻烦，因为想要在网页上播放Flash需要一堆代码和插件。<canvas>标签使得开发者只要使用一个标签就能和用户产生UI交互。虽然目前<canvas>标签还不能实现Flash的所有功能，但是相信很快就会颠覆并代替flash。

4.新的<header>与<footer>标记
HTML5的设计是要更好的描绘网站的解剖结构。这就是为什么一些像<header>和 <footer>这样的新标记会出现，它们是专门为标志网站的这些部分设计的。用来明确表示网页的结构。

5.新的<section>与<article>与标记
跟<header>与<footer>标记相似，HTML5中引入的新的<section>和<article>标记可以让开发人员更好的标注页面上的这些区域。有利于清晰化网页的结构，更有利于SEO。

6.新的<menu>和<figure> 标记
新的<menu>可以被用于创建传统的菜单，也可以用于工具栏和上下文菜单。新的< figure>标签使得网页文字和图片的排版更专业。

7.新的<audio>和<video>标记
新的<audio>和<video>标记可能是HTML5中增加的最有用处的两个东西了。正如标记名称，它们是用来嵌入音频和视频文件的。
除此之外还增加了新的多媒体的标记和属性，例如<track>，它是用来提供跟踪视频的文字信息的。

8.表单的全新水平
新的 <form>和<forminput> 标记对原有的表单元素进行的全新的修改，添加了很多的新属性，也修改了很多属性。如果你经常的开发表单，建议花时间更详细的研究一下。

9.不再使用<b> 和 <font>标记
官方说明是这些标记可以通过CSS来做更好的处理，也许我们以后会习惯这种方法。

10.不再使用<frame>, <center>, <big>标记
有了更好的标记能实现他们的功能。

这10个HTML5和HTML4之间的不同只是整个新的规范中的一小部分。除了这些主要的变动外，我还可以略提一下一些次要的改动，比如修改了<ol>标记的属性，让它能够倒排序.
```

# 语义化标签

```
常用的语义化标签
<hgroup></hgroup>
<header></header>
<nav></nav>
<section></section>
<footer></footer>
<article></article>
<aside></aside>
```

## hgroup
hgroup元素代表网页或section的标题，当元素有多个层级时，该元素可以将h1到h6元素放在其内，譬如文章的主标题和副标题组合
例如：
```
<hgroup>
    <h1>网站标题</h1>
    <h2>网站副标题</h2>
</hgroup>
```
注意：连续多个h标签才用hgroup

## header
header元素代表网页或section的页眉
通常包含h标签或hgroup
例如：
```
<header>
    <hgroup>
        <h1>网站标题</h1>
        <h2>网站副标题</h2>
    </hgroup>
<header>
```
注意：没有个数限制，如果hgroup或h标签自己就能工作得很好，就不需要使用header

## nav
nav元素代表页面的导航链接区域，用于定义页面的主要导航部分
例如：
```
<nav>
    <ul>
        <li>HTML5</li>
        <li>CSS</li>
        <li>JavaScript</li>
    </ul>
</nav>
```
注意：用在整个页面主要导航上，不合适就不要用

## section
section元素代表文档中的节或段，段可以是一篇文章里按照主题的分段，节可以是指一个页面里的分组
注意：section不是一般意义上的容器元素，如果想作为样式展示和脚本的便利，可以用div
article，nav，aside可以理解为特殊的section，可以用article，nav，aside就不要用section

## article
article元素最容易跟section和div混淆，其实article代表一个在文档，页面或者网站中自成一体的内容

## aside
aside元素包含在article元素中作为主要内容的附属信息部分，其中的内容可以是与当前文章有关的相关资料，标签，名词解释等
在article元素之外使用作为页面或站点全局的附属信息部分，最典型的是侧边栏，其中的内容可以是日志串连，其他组的导航

# canvas画布

## canvas基础点

canvas是h5新增的元素，可通过JS来绘制图形
例如，它可以用于绘制图形，创建图画
默认width:300px,height:150px
默认颜色为body背景色
IE9以下不支持
注意：
html属性设置宽高时只影响画布本身不影响画布内容
css属性设置宽高时不但会影响画布本身的宽高，还会使画布中的内容等比例缩放（缩放参照于画布默认的尺寸）

 1.路径容器
每次调用路径api时,都会往路径容器里做登记
调用beginPath时,清空整个路径容器

2.样式容器
每次调用样式api时,都会往样式容器里做登记
调用save时候,将样式容器里的状态压入样式栈
调用restore时候,将样式栈的栈顶状态弹出到样式容器里,进行覆盖

3.样式栈
调用save时候,将样式容器里的状态压入样式栈
调用restore时候,将样式栈的栈顶状态弹出到样式样式容器里,进行覆盖


## canvas的基本用法

canvas元素只会创造一个固定大小的画布，要在上面绘制内容，我们需要找到它的渲染上下文‘
canvas元素有一个叫getContext()的方法，这个方法是用来获得渲染上下文和它的绘画功能
getContext()只有一个参数，上下文的格式
例如：
```
var canvas = document.getElementById("box");
if (canvas.getContext) {
    var ctx = canvas.getContext("2d");`
}

```

## canvas绘制矩形

canvas元素只支持一种原生的图形绘制：矩形
所有其他的图形的绘制都至少需要生成一条路径

绘制矩形
    canvas提供了三种方式绘制矩形：
    1.绘制一个填充的矩形（填充色默认为黑色）
    `fillRect(x, y, width, height)`
    2.绘制一个矩形的边框（默认边框为：1像素实心黑色）
    `strokeRect(x, y, width, height)`
    3.清除指定矩形区域，让清除部分完全透明
    `clearRect(x, y, width, height)`

x和y制定了在canvas画布上所绘制的矩形的左上角相对于原点的坐标不加单位
width和height设置矩形的尺寸（存在边框的话，边框会在width上占据一个边框的宽度，height同理）不加单位

strokeRect边框像素渲染问题
    canvas在渲染矩形边框时，边框宽度是平均分在偏移位置两侧，会渲染成2px

## 添加样式和颜色

fillStyle：设置图形的填充颜色
例如：`ctx.fillStyle = "red";`
strokeStyle：设置图形轮廓的颜色
例如：`ctx.strokeStyle = "pink";`
默认情况下，边框和填充色都是黑色

lineWidth：设置绘线的粗细，属性值必须为正数
0，负数，Infinit，NaN会被忽略
例如：`ctx.lineWidth = 2;`

lineJoin：设定线条与线条间结合处的样式（默认是miter）
round：圆角
bevel：斜角
miter：直角

ctx.lineCap = "butt";
lineCap指定绘制每一条线段末端属性默认值为butt
三个属性值：
butt :线段末端以方形结束
round :线段末端以圆形结束
square :线段末端以方形结束，增加了一个宽度与线段相同，高度是线段厚度一半的矩形区域

globalAlpha = value
这个属性影响canvas里所有图形的透明度
有效值为0.0（完全透明）到1.0（完全不透明）
默认值为1.0

save():样式压栈
restore():样式弹栈

## 绘制路径

1.首先，需要创建路径的起始点
2.使用画图命令画出路径
3.将路径封闭
4.一旦路径形成，通过描边或填充路径区域来渲染图形

```
    ctx.moveTo(50, 100);
    ctx.lineTo(50, 150);
    ctx.lineTo(100, 150);
    ctx.closePath(); //closepath自动闭合路径
    ctx.stroke(); //不lineto原点或closePath无法自动闭合路径
    ctx.fill();  //fill方法会自动闭合路径并填充
    ctx.beginPath();//清空路径容器
    ctx.moveTo(100, 150);
    ctx.lineTo(100, 200);
    ctx.lineTo(150, 200);
    ctx.closePath(); //closepath自动闭合路径
    ctx.stroke(); //不会自动调用closePath
    ctx.fill();  //fill方法会自动调用closePath并填充
```

## canvas基本模板

1.路径容器：
    每次调用路径API时，都会往路径容器做登记
    使用beginPath，清空整个路径容器

2.样式容器:
    每次调用样式API时，都会往样式容器做标记
    调用save时，将样式容器里的状态压入样式栈
    调用restore时，将样式栈栈顶状态弹入样式容器覆盖

```
ctx.save();
ctx.beginPath();
ctx.restore();
```

## canvas签名

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>canvas签名</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        #canvas {
            position: absolute;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            margin: auto;
            background-color: azure;
        }
    </style>
</head>
<body>
    <canvas id="canvas" height="500" width="500">
        建议使用IE8以上或Chrome,FireFox浏览器！
    </canvas>
    <script>
        var canvas = document.getElementById("canvas");
        if (canvas.getContext) {
            var ctx = canvas.getContext("2d");
        }
        canvas.onmousedown = function (ev) {
            ev = ev || window.event;
            canvas.setCapture && canvas.setCapture();
            ctx.beginPath();
            ctx.moveTo(ev.clientX - canvas.offsetLeft, ev.clientY - canvas.offsetTop);
            document.onmousemove = function (ev) {
                ev = ev || window.event;
                ctx.save();
                ctx.strokeStyle = "red";
                ctx.lineTo(ev.clientX - canvas.offsetLeft, ev.clientY - canvas.offsetTop);
                ctx.stroke();
                ctx.restore();
            };
            document.onmouseup = function () {
                document.onmousemove = document.onmouseup = null;
                canvas.releaseCapture && canvas.releaseCapture();

            };
            return false;
        }
    </script>
</body>
</html>
```

## canvas曲线

### 绘制圆形

`arc(x, y, radius, startAngle, endAngle, anticlockwise)`
画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始至endAngle结束（单位为弧度），按照anticlockwise给定方向来生成，anticlockwise为布尔值（默认逆时针，true为逆时针，false为顺时针）
例如：
```
var canvas = document.getElementById("test");
if (canvas.getContext) {
    var ctx = canvas.getContext("2d");
    ctx.beginPath();
    ctx.arc(100, 100, 50, 0, 2 * Math.PI, true);
    ctx.stroke();
}
```

`arcTo(x1, y1, x2, y2, radius)`
根据给定的控制点和半径画一段圆弧，
肯定会经过（x1, y1），但不一定经过（x2, y2）,（x2, y2）只控制方向

例如：
```
if (canvas.getContext) {
    var ctx = canvas.getContext("2d");
    ctx.beginPath();
    ctx.moveTo(100, 100);
    ctx.arcTo(150, 300, 200, 100, 10);
    ctx.stroke();
    ctx.beginPath();
    ctx.moveTo(100, 100);
    ctx.lineTo(150, 300);
    ctx.lineTo(200, 100);
    ctx.stroke();
}
```

### 绘制贝塞尔曲线

二次贝塞尔曲线

`quadraticCurveTo(cp1x, cp1y, x, y)`
绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点，起始点为moveTo指定的点


三次贝塞尔曲线

`bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`
绘制三次贝塞尔曲线，cp1x, cp1y为控制点一，cp2x, cp2y为控制点二，x, y为结束点，起始点为moveTo指定的点

## canvas图像变换

translate(x, y)
用来移动canvas的原点，x是右偏移量，y是上偏移量，在canvas中translate是累加的（原点相对运动，画布不运动）

rotate(angle)
只接受一个参数，旋转的角度（angle），顺时针方向，以弧度为单位，旋转的中心点始终是canvas的原点，如果需要改变原点，需要用到translate方法，在canvas中rotate是累加的

scale(x, y)
两个参数x, y分别是横轴和纵轴的缩放因子，都必须是正值，值比1小表示缩小，值比1大表示放大，值为1不变，相当于缩小或放大CSS像素的面积，在canvas中scale是累加的。

save压栈，保存到栈中的绘制状态组成
1.当前的变换矩阵
2.当前的剪切区域
3.当前的虚线列表
4.样式strokeStyle,fillStyle,lineWidth,lineCap,lineJoin

图像变换实例
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>canvas图像变换</title>
    <style>
        #test {
            position: absolute;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            margin: auto;
            background-color: #d6e9c6;

        }
    </style>
</head>
<body>
    <canvas id="test" width="500" height="500">
    建议使用IE8以上或Chrome,Firefox浏览器
    </canvas>
    <script>
        var canvas = document.getElementById("test");
        var flag = 0;
        var change = 0;
        var changeFlag = 0;
        if (canvas.getContext) {
            var ctx = canvas.getContext("2d");
            var timer = setInterval(function () {
                if (change === 0) {
                    changeFlag = 1;
                }
                else if (change === 200) {
                    changeFlag = -1;
                }
                change += changeFlag;
                flag++;
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                ctx.save();
                ctx.translate(250, 250);
                ctx.rotate(flag * Math.PI /180);
                ctx.scale(change / 100, change / 100);
                ctx.beginPath();
                ctx.fillRect(-50, -50, 100, 100);
                ctx.restore();
            }, 1)
        }
    </script>

</body>
</html>
```

## canvas引入图片

### 引入图片

在canvas中插入图片（需要image对象）
1.canvas操作图片时，必须等图片加载完才能操作
2.drawImage(image, x, y, width, height)
其中image是image或者canvas对象，x和y是其在目标canvas里的起始坐标，width和height控制canvas画入时缩放的大小

```
var canvas = document.getElementById("test");
if (canvas.getContext) {
    var ctx = canvas.getContext("2d");
    var img = new Image();
    img.src = "img/1.jpg";
    img.onload = function () {
        draw();
    }
}
function draw() {
    ctx.drawImage(img, 0, 0, img.width, img.height);
}
```

### 设置图片背景

给canvas设置背景（需要image对象）
1.canvas操作图片时，必须等图片加载完才能操作
2.createPattern(image, repetition)
    image：图像源
    repetition："repeat"
                       "repeat-x"
                       "repeat-y"
                       "no-repeat"
一般情况下，我们都会将createPattern返回的对象作为fillstyle的值

```
var canvas = document.getElementById("test");
if (canvas.getContext) {
    var ctx = canvas.getContext("2d");
    var img = new Image();
    img.src = "img/2.jpg";
    img.onload = function () {
        draw();
    }
}
function draw() {
    var pattern = ctx.createPattern(img, "no-repeat");
    ctx.fillStyle = pattern;
    ctx.fillRect(0, 0, 500, 500);
}
```

## canvas渐变

### 线性渐变

createLinearGradient(x1, y1, x2, y2)
表示渐变的起点(x1, y1)与终点(x2, y2)

gradient.addColorStop(position, color)
gradient：createLinearGradient(x1, y1, x2, y2)的返回值
addColorStop 方法接受2个参数：
    position：参数必须是一个0~1之间的数值，表示渐变中颜色所在的相对位置，例如0.5表示颜色会出现在正中间‘
    color必须是一个有效的CSS颜色值，例如"#FFF""red"等

```
var canvas = document.getElementById("test");
if (canvas.getContext) {
    var ctx = canvas.getContext("2d");
    var gradient = ctx.createLinearGradient(0, 0, 300, 300);
    gradient.addColorStop(0, "#bfa");
    gradient.addColorStop(0.3, "black");
    gradient.addColorStop(0.5, "red");
    gradient.addColorStop(0.8, "pink");
    ctx.fillStyle = gradient;
    ctx.fillRect(0, 0, 300, 300);
}
```

### 径向渐变

createRadialGradient(x1, y1, r1, x2, y2, r2)
前三个参数定义一个以(x1, y1)为原点，半径为r1的圆
后三个参数定义另一个以(x2, y2)为原点，半径为r2的圆

## canvas文本相关

### 绘制文本

fillText(text, x,  y)
在指定的(x, y)位置填充指定的文本

strokeText(text, x, y)
在指定的(x, y)位置绘制文本边框

### 文本样式

font = "value"
绘制文本样式的字符串使用和CSS font属性语法相同，默认字体"10px sans-serif",font属性在指定时，必须要有大小和字体缺一不可

textAlign = "value"
文本对齐选项，可选值left, right, center
比较特殊，文本的对齐是基于你在fillText的时候所绘制的x的值，也就是说文本一半在x左，一半在x右

textBaseline = "value"
描述绘制文本时，当前文本基线的属性
top: 文本基线在文本的顶部
middle: 文本基线在文本中间
bottom: 文本基线在文本底部

measureText()
该方法返回一个TextMetrics对象，包含关于文本尺寸的信息，例如文本宽度
ctx.measureText("文本").width;

### 文本水平居中

```
var canvas = document.getElementById("test");
if (canvas.getContext) {
    var ctx = canvas.getContext("2d");
    ctx.font = "50px impact";
    ctx.textBaseline = "middle";
    var w = ctx.measureText("文本水平垂直居中").width;
    ctx.fillText("文本水平垂直居中", (canvas.width - w) / 2, (canvas.height - 50) / 2);
}
```
### 文本阴影

shadowOffsetX = float;
设置阴影在x轴延伸距离，默认为0；

shadowOffsetY = float;
设置阴影在y轴延伸距离，默认为0；

shadowBlur = float;
用于设定阴影的模糊程度，其数值不跟像素数量挂钩，也不受变换矩阵影响，默认为0；

shadowColor = "value";
标准的CSS颜色值，用于设定阴影颜色效果，默认透明黑色；

## canvas像素操作

### ImageDate对象

ImageData对象中存储着canvas对象真实的像素数据，它包含以下几个只读属性
width: 图片宽度，单位像素
height: 图片高度，单位像素
date: Uint8ClampedArray类型的一维数组
包含每个像素点的RGBA格式的整型数据，范围0~255
R/G/B: 0→255（黑色到白色）
A：0→255（透明到不透明）

### 获取场景像素数据

getImageData()
获得一个包含画布场景像素数据的ImageData对象，它代表了画布区域的对象数据

ctx.getImageData(x, y, width, height)
x: 将要被提取的图像数据矩形区域的左上角x坐标
y: 将要被提取的图像数据矩形区域的左上角y坐标
width: 将要被提取的图像数据矩形区域的宽度
height: 将要被提取的图像数据矩形区域的高度

### 在场景中写入像素数据

putImageData()方法对场景进行像素数据写入
putImageData(myImageDate, x, y)
x和y参数表示你希望在场景内左上角绘制的像素数据所得坐标 

### 创建一个ImageDate对象

createImageData(width, height)
width: ImageDate新对象宽度
height:ImageDate新对象高度
默认创建出来是透明的

### 单像素操作

```
function getPxInfo(imageData, x, y) {
    var rgba = [];
    var w = imageData.width;
    rgba[0] = imageData.data[(y * w + x) * 4];
    rgba[1] = imageData.data[(y * w + x) * 4 + 1];
    rgba[2] = imageData.data[(y * w + x) * 4 + 2];
    rgba[3] = imageData.data[(y * w + x) * 4 + 3];
    return rgba;
}
```

```
function setPxInfo(imageData, x, y, rgba) {
    var w = imageData.width;
    imageData.data[(y * w + x) * 4] = rgba[0];
    imageData.data[(y * w + x) * 4 + 1] = rgba[1];
    imageData.data[(y * w + x) * 4 + 2] = rgba[2];
    imageData.data[(y * w + x) * 4 + 3] = rgba[3];
}
```

### 马赛克

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        #test {
            background-color: #c4e3f3;
            position: absolute;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            margin: auto;
        }
    </style>
</head>
<body>
<canvas id="test">
    建议使用IE8以上或Chrome,Firefox浏览器
</canvas>
<script>
    var canvas = document.getElementById("test");
    if (canvas.getContext) {
        var ctx = canvas.getContext("2d");
        var image = new Image();
        image.src = "../img/4.jpg";
        image.onload = function () {
            canvas.width = image.width * 2;
            canvas.height = image.height;
            draw();
        };
        function draw() {
            ctx.drawImage(image, 0, 0);
            var oldImageData = ctx.getImageData(0, 0, image.width, image.height);
            var newImageData = ctx.createImageData(image.width, image.height);
            var size = 5;
            for (var i = 0; i < oldImageData.width / size; i++) {
                for (var j = 0; j < oldImageData.height / size; j++) {
                    var color = getPxInfo(oldImageData, i * size + Math.floor(Math.random() * size), j * size + Math.floor(Math.random() * size));
                    for (var m = 0; m < size; m++) {
                        for (var n = 0; n < size; n++) {
                            setPxInfo(newImageData, i * size + m, j * size + n, color);
                        }
                    }
                }
            }
            ctx.putImageData(newImageData, image.width, 0);
        }
        function getPxInfo(imageData, x, y) {
            var rgba = [];
            var w = imageData.width;
            rgba[0] = imageData.data[(y * w + x) * 4];
            rgba[1] = imageData.data[(y * w + x) * 4 + 1];
            rgba[2] = imageData.data[(y * w + x) * 4 + 2];
            rgba[3] = imageData.data[(y * w + x) * 4 + 3];
            return rgba;
        }
        function setPxInfo(imageData, x, y, rgba) {
            var w = imageData.width;
            imageData.data[(y * w + x) * 4] = rgba[0];
            imageData.data[(y * w + x) * 4 + 1] = rgba[1];
            imageData.data[(y * w + x) * 4 + 2] = rgba[2];
            imageData.data[(y * w + x) * 4 + 3] = rgba[3];
        }
    }


</script>
</body>

</html>
```


## canvas合成

source:新的图像（源）
destination:已经绘制过的图形（目标）

globalCompositeOperation = "value"
可选值：
    source-over(默认值)：源在上面，新的图像层级比较高
    source-in:只留下源与目标的重叠部分（源的那一部分）
    source-out:只留下源超过目标的部分
    source-atop:砍掉源溢出的部分
    destination-over:目标在上面，旧的图像层级比较高
    destination-in:只留下源与目标的重叠部分（目标的那一部分）
    destination-out:只留下目标超过源的部分
    destination-atop:砍掉目标溢出的部分

### 刮刮卡

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no">
    <title>Title</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }
        html, body {
            height: 100%;
            overflow: hidden;
        }
        #wrap, ul, li {
            height: 100%;
        }
        li {
            background: url("../img/12.jpg");
            background-size: 100%, 100%;
        }
        canvas {
            position: absolute;
            top: 0;
            left: 0;
            transition: 1s;
        }

    </style>
</head>
<body>
    <div id="wrap">
        <canvas id="canvas"></canvas>
        <ul>
            <li>
            </li>
        </ul>
    </div>
    <script>
        var canvas = document.getElementById("canvas");
        canvas.width = document.documentElement.clientWidth;
        canvas.height = document.documentElement.clientHeight;
        if (canvas.getContext) {
            var ctx = canvas.getContext("2d");
            var img = new Image();
            img.src = "../img/timg-22.jpeg";
            img.onload = function () {
                draw();
            };
            function draw() {
                var Px = 0;
                ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
                canvas.addEventListener("touchstart", function (ev) {
                    ev = ev || event;

                    var touchC = ev.changedTouches[0];

                    var x = touchC.clientX - canvas.offsetLeft;
                    var y = touchC.clientY - canvas.offsetTop;
                    ctx.globalCompositeOperation = "destination-out";
                    ctx.lineWidth = 40;
                    ctx.lineCap = "round";
                    ctx.lineJoin = "round";
                    ctx.save();
                    ctx.beginPath();
                    ctx.moveTo(x, y);
                    ctx.stroke();
                    ctx.restore();

                }, false);
                canvas.addEventListener("touchmove", function (ev) {
                    ev = ev || event;

                    var touchC = ev.changedTouches[0];

                    var x = touchC.clientX - canvas.offsetLeft;
                    var y = touchC.clientY - canvas.offsetTop;

                    ctx.save();
                    ctx.lineTo(x + 1, y + 1);
                    ctx.stroke();
                    ctx.restore();

                }, false);
                canvas.addEventListener("touchend", function () {
                    var imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);
                    var allPx = imgData.width * imgData.height;
                    for (var i = 0; i < allPx; i++) {
                        if (imgData.data[4 * i + 3] === 0) {
                           Px++;
                        }
                    }
                    if (Px >= allPx / 2) {
                        canvas.style.opacity = 0;
                    }
                }, false);
                canvas.addEventListener("transitionend", function () {
                    this.remove();
                }, false);
            }
        }
    </script>
</body>
</html>

```

## 其他

### 将画布导出为图像

toDataURL(注意是canvas元素接口上的方法)
canvas.toDataURL();拿到canvas图像地址

### canvas内图像事件操作

ctx.isPointInPath(x, y);
判断在当前路径中是否包含检测点
注意：此方法只作用于最新画出的canvas图像

# H5音视频标签

```
<video>:html5提供播放视频的标签
属性：
    src:资源地址
    controls:该属性定义显示还是隐藏用户控制界面

<audio>:html5提供播放音频的标签
属性：
    src:资源地址
    controls:该属性定义显示还是隐藏用户控制界面

<source>
视频属性：
    src:资源地址
    type='video/webm; codecs="vp8, vorbis"'
    type='video/ogg; codecs=theora, vorbis"'
    type='video/mp4; codecs="avc1.42E01E, mp4a.40.2"'

音频属性：
    src:资源地址
    type='audio/ogg; codecs="vorbis"'
    type='audio/aac; codecs="aac"'
    type='audio/mpeg;'
```

## video标签属性

width：视频显示区域的宽度，单位是CSS像素
height：视频显示区域的高度，单位是CSS像素
poster：一个海报的URL，用于在用户播放或者跳帧之前展示
src：要嵌到页面的视频的URL

controls：显示或隐藏用户控制界面
autoplay：媒体是否自动播放
loop：媒体是否循环播放
muted：是否静音

preload：该属性旨在告诉浏览器作者认为达到最佳的用户体验的方式是什么
可选值：
    none：提示作者认为用户不需要查看该视频，服务器也想要最小化访问流量；换句话说就是提示浏览器该视频不需要缓存
    metadata：提示尽管作者认为用户不需要查看该视频，不过抓取元数据（比如：长度）还是很合理的
    auto：用户需要这个视频优先加载；换句话说就是提示：如果需要的话，可以下载整个视频，即使用户不一定会用它
    空字符串：也就代指auto

## audio标签属性

src controls autoplay loop muted preload

## 音视频JS相关属性

duration : 媒体总时间（只读）
currentTime : 开始到播放现在所用的时间（可读写）
muted : 是否静音（可读写，相比于volume优先级要高）
volume : 0.0~1.0的音量相对值（可读写）
注意muted与volume不同步
paused : 媒体是否暂停（只读）
ended : 媒体是否播放完毕（只读）
error : 媒体发生错误的时候，返回错误代码（只读）
currentSrc : 以字符串的形式返回媒体地址（只读）

视频标签特有：
poster : 视频播放前的预览图片（可读写）
width\height : 设置video视频标签的尺寸（可读写）
videoWidth\videoHeight : 视频的实际尺寸（只读）

## 音视频JS相关方法

play() ：媒体播放
pause() : 媒体暂停
load() : 重新加载媒体（用source.src时）


# 其他新增标签

## 状态标签

### meter

meter:用来显示已知范围的标量值或者分数值（类似于电池显示）。
value:当前的数值。
min:值域的最小边界值。如果设置了，它必须比最大值要小。如果没设置，默认为0
max:值域的上限边界值。如果设置了，它必须比最小值要大。如果没设置，默认为1
low:定义了低值区间的上限值,如果设置了，它必须比最小值属性大，并且不能超过high值和最大值。未设置或者比最小值还要小时，其值即为最小值。
high:定义了高值区间的下限值。如果设置了，它必须小于最大值，同时必须大于low值和最小值。如果没有设置，或者比最大值还大，其值即为最大值。
optimum:这个属性用来指示最优/最佳取值。

### progress

progress:用来显示一项任务的完成进度
max:该属性描述了这个progress元素所表示的任务一共需要完成多少工作.
value：该属性用来指定该进度条已完成的工作量.
如果没有value属性,则该进度条的进度为"不确定",
也就是说,进度条不会显示任何进度,你无法估计当前的工作会在何时完成

## 列表标签

### datalist

datalist会包含一组option元素，这些元素表示其表单控件的可选值
它的id必须要和input中的list一致

### details

 一个ui小部件，用户可以从其中检索附加信息。
open属性来控制附加信息的显示与隐藏
summary:用作 一个`<details>`元素的一个内容摘要（标题）

## 注释标签

### ruby

rt: ruby的子标签，展示文字注音或字符注释。

## 标记标签

### mark

mark:着重

# HTML基础表单

1.表单仍然使用`<form>`元素作为容器，我们可以在其中设置基本的提交特性
form的action指向一个服务器地址（接口）

2.当用户和开发人员提交页面时，表单仍然用于向服务端发送表单中控件的值
注意表单项的name属性必须有值，服务器才能获取表单

3.所有之前的表单控件都保持不变

4.仍然可以使用脚本操作表单控件

5.HTML5之前的表单

标签为input
type:text:文本框
type:password:密码框

type:radio:单选按钮
注意以name分组，确保单选关系，也为了后台能成功获取
必须有value属性，为了后台获取后的识别（不写统一为on）
checked属性,选中控制

type:checkbox:复选框
注意以name分组，确保为一组，也为了后台能成功获取
必须有value属性，为了后台获取后的识别（不写统一为on）
checked属性,选中控制

type:submit:提交按钮
type:reset:重置按钮
type:button:普通按钮

标签为select:下拉框
name属性在select标签上
multiple:可选多项
子项可以通过optgroup来进行分组
label属性用来定义组名
子项为option标签
selected属性,选中控制
必须有value属性,为了后台获取后的识别

标签为textarea:文本域

标签为button:按钮
type:submit:提交按钮
type:reset:重置按钮
type:button:普通按钮

标签为lable:控制文本与表单的关系
for属性指向一个input的id

标签fieldset 标签legend:来为表单分组	

6.attr & prop

# HTML5新增表单

1.type:email :email地址类型
当格式不符合email格式时，提交是不会成功的，会出现提示；只有当格式相符时，提交才会通过
在移动端获焦的时候会切换到英文键盘

2.type:tel :电话类型
在移动端获焦的时候会切换到数字键盘

3.type:url :url类型
当格式不符合url格式时，提交是不会成功的，会出现提示；只有当格式相符时，提交才会通过

4.type:search :搜索类型
有清空文本的按钮

5.type:range  :  特定范围内的数值选择器
属性:min、max、step

6.
type:number          :  只能包含数字的输入框
type:color  	       		:  颜色选择器
type:datetime        	 :  显示完整日期(移动端浏览器支持)
type:datetime-local  :  显示完整日期，不含时区
type:time            :  显示时间，不含时区
type:date            :  显示日期
type:week            :  显示周
type:month           :  显示月

# HTML5新增表单属性

placeholder : 输入框提示信息
适用于form,以及type为text,search,url,tel,email,password类型的input
怎么选中placeholder？
伪元素::-webkit-input-placeholder

autofocus  :  指定表单获取输入焦点

required  :  此项必填，不能为空

pattern : 正则验证  pattern="\d{1,5}\"

formaction：在submit里定义提交地址

list和datalist  :  为输入框构造一个选择列表
input的list值为datalist标签的id

# 表单验证反馈

validity对象，通过下面的valid可以查看验证是否通过，如果八种验证都通过返回true，一种验证失败返回false
node.addEventListener("invalid",fn1,false);

valueMissing  	 :  输入值为空时返回true
typeMismatch 	 :  控件值与预期类型不匹配返回true
patternMismatch  :  输入值不满足pattern正则返回true

tooLong  		 :  超过maxLength最大限制返回true
rangeUnderflow   :  验证的range最小值返回true
rangeOverflow    :  验证的range最大值返回true
stepMismatch     :  验证range 的当前值 是否符合min、max及step的规则返回true

customError     ：不符合自定义验证返回true
setCustomValidity("")

关闭验证
formnovalidate属性
