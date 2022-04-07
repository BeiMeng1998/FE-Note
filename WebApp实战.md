# 像素理论

## 屏幕大小

屏幕对角线长度

## 屏幕分辨率

横纵向上像素点的个数，单位是px，1px为一个像素点（物理像素）

物理像素：设备呈像的最小单位
CSS像素：浏览器中的最小单位
位图像素：图像的最小单位

物理像素与CSS像素的关系
一个CSS像素占据多少个物理像素和屏幕有关

## 屏幕像素密度PPI

对角线上一英寸像素点的个数

## 设备独立像素

是设备对接CSS像素的接口
这个点代表一个可以由程序使用的虚拟像素（比如CSS像素），然后由相关系统转换为物理像素

## 像素比DPR

设备物理像素和设备独立像素的比例
也就是devicePixelRatio = 物理像素个数/设备独立像素个数
可以通过window.devicePixelRatio来获取

# 视口理论

## 布局视口

let layout = document.documentElement.clientWidth

## 视觉视口

视觉视口决定用户能看到什么

let visual = window.innerWidth

## 理想视口

布局视口 = 视觉视口

let dream = screen.width (有很大的兼容性问题

# 用户缩放

改变CSS像素的面积

PC端放大缩小视口改变布局视口

移动端放大缩小视口改变视觉视口不改变布局视口

# 系统缩放

meta标签的initial-scale=1.0

系统缩放改变布局视口和视觉视口，相对于理想视口进行缩放

# 完美视口

`<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no" />`

完美视口不仅仅只能解决旋转时的问题。
如果你页面中存在太大的元素，你的meta标签只使用width=device-width，initial-scale=1.0中的
一个，有些浏览器会扩展布局视口的宽度来容纳这个元素，这里的兼容性很复杂，但你两个都使用了，
大部分浏览器都不会改变布局视口了

问题：完美视口下不同设备下网页大小相同

# rem适配

em相当于自身的font-size
rem相当于根标签的font-size
chrome字体默认16px，最小12px

rem适配原理：不改变CSS像素大小，改变元素在不同设备上所占CSS像素的个数
优点：没有破坏完美视口
缺点：px到rem转换太复杂
```
//满屏16rem适配
(function () {
    const styleNode = document.createElement("style");
    const w = document.documentElement.clientWidth / 16;
    styleNode.innerHTML = `html{font-size:${w}px!important;`;
    document.head.appendChild(styleNode);
})();

```

# viewport适配

viewport适配原理：不改变元素所占CSS像素个数，改变CSS像素和物理像素的比例
优点：所量即所得
缺点：没有使用完美视口

```
<meta name="viewport" content="width=device-width" />

(function () {
    const targetWidth = 750;
    const scale = document.documentElement.clientWidth / targetWidth;
    const meta = document.querySelector("meta[name='viewport']");
    meta.content=`initial-scale=${scale},minimum-scale=${scale},maximum-scale=${scale},user-scalable=no`;
})();
```

# 实现CSS像素=物理像素

## rem适配实现（淘宝）

```
    window.onload = function () {
        (function () {
            let dpr = window.devicePixelRatio || 1
            let styleNode = document.createElement("style")
            let w = document.documentElement.clientWidth * dpr / 16
            styleNode.innerHTML = `html {font-size:${w}px!important;}`
            document.head.appendChild(styleNode)

            let scale = 1 / dpr
            let meta = document.querySelector("meta[name='viewport']")
            meta.content = `width=device-width, initial-scale=${scale}`
        })()
    }
```

## 媒体查询实现

```
@media only screen and (-webkit-device-pixel-ratio:2) {
    #test {
        transform: scaleY(0.5);
    }
}

@media only screen and (-webkit-device-pixel-ratio:3) {
    #test {
        transform: scaleY(0.333);
    }
}
```

# 移动事件基础

## 触屏事件

### touchstart touchmove touchend

与PC端 onmousedown onmousemove onmouseup 的区别：
    touchmove不可能单独触发，onmousemove可以单独触发

通过addEventListener添加监听

## 全面禁止事件的默认点击行为

touchstart
ev.preventDefault()
可通过ev.stopPropagation()保留默认行为

隐患：页面所有的滚动条失效

## 事件点透

1.PC端click事件在移动端可以触发
2.移动端touchend事件与PC端click事件有300-350ms的延迟，判断用户是否会进行双击手势用以缩放文字


### 移动端a标签跳转方案

```
    let aNodes = document.getElementsByTagName("a")
    for (let index = 0; index < aNodes.length; index++) {
        aNodes[index].addEventListener("touchstart", function () {
            this.isMoved = false
            this.addEventListener("touchmove", function () {
                this.isMoved = true
            })
        })
        aNodes[index].addEventListener("touchend", function () {
            if (!this.isMoved) {
                location = this.href
            }
        })
    }
```

## 移动端event手指列表

### changedTouches

触发当前事件的手指列表

### targetTouches

触发当前事件时元素上的手指列表

### touches

触发当前时间时，屏幕上的手指列表

# 移动端常见问题

## 电话和邮箱问题

` <meta name="format-detection" content="telephone=no,email=no"> `

`<a href="tel:110">110</a>`
`<a href="mailto:1032916702@qq.com">1032916702@qq.com</a>`

## 链接按钮高亮问题

```
a {
    text-decoration: none;
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
```

## border圆角问题

```
input {
    width: 50px;
    height: 50px;
    border-radius: 5px;
    -webkit-appearance: none;
}
```

## Font Boosting

Font Boosting是Webkit给移动端浏览器提供的一个特性，当我们在手机上浏览网页时，很可能因为手机屏幕缩小后看不清其中文字，而Font Boosting特性在这时会自动将其中文字字体变大

解决方案
max-height: 999999px;
