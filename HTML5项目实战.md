# DOM位置

## parentNode

parentNode：元素的父节点

## offsetParent

offsetParent：
    元素本身定位为fixed：offsetParent:null（除火狐）offsetParent:body（火狐）
    元素本身定位不为fixed：offsetParent为最先开启定位的祖先元素；若祖先没有定位，则offsetParent:body

body的offsetParent为null

## offsetLeft/offsetTop

offsetLeft和offsetTop是参照于offsetParent的内边距边界的，即padding-box

DOM里所有的元素都是有offsetLeft和offsetTop的

body的offsetLeft和offsetTop为0

## getBoundingClientRect()

元素的相对位置

返回一个对象
对象属性包括height,width,left,top,right,bottom

height,width代表元素border-box尺寸

left,top代表元素左上角的相对位置

right,bottom代表元素右下角的相对位置


## 元素的绝对位置

相对位置 + scrollTop/scrollLeft

注意：scrollTop/scrollLeft并不是滚动条滚动的距离而是页面元素滚动的距离

# DOM尺寸

## clientWidth/Hight

padding-box（可视区域）的宽高

注意 document.documentElement.clientWidth/Height并不是根标签的可视区域，就是视口的大小

## offsetWidth/Hight

border-box 的宽高

注意 document.documentElement.offsetWidth/Height还是是根标签的border-box的大小
在IE10及以下，document.documentElement.client/offsetWidth/Height统一指定为视口的宽高

# 事件的传播/事件流

事件的传播分为三个阶段

1.捕获阶段
在捕获阶段时从最外层的祖先元素，向目标元素进行事件的捕获，但是默认此时不会触发事件

2.目标阶段
事件捕获到目标元素，捕获结束开始在目标元素上触发事件
如果希望在捕获阶段就触发事件，可以将addEventListener()的第三个参数设置为true，一般情况下我们不会希望在捕获阶段触发事件，所以这个参数一般都是false

3.冒泡阶段
事件从目标元素向它的祖先元素传递，依次触发祖先元素上的事件
事件的冒泡（Bubble）
所谓的事件的冒泡指的是事件的向上传导，当后代元素上的事件被触发时，其祖先元素的相同事件也会被触发
在开发中大部分情况冒泡都是有用的，如果不希望发生事件冒泡，可以通过事件对象来取消冒泡
event = event || window.event;
event.stopPropagation();

IE8及以下的浏览器中没有捕获阶段

# 事件的绑定

DOM0
使用 对象.事件 = 函数 的形式绑定函数
它只能同时为一个元素的一个事件绑定一个响应函数
不能绑定多个，如果绑定了多个，则后面会覆盖前面的

DOM2
addEventListener()
通过这个方法也可以为元素绑定响应函数
参数：
1.事件的字符串，不要on
2.回调函数，当事件触发时该函数会被调用
3.是否在捕获阶段触发事件，需要一个布尔值，一般都传false
使用这种方式可以同时为一个元素的相同事件同时绑定多个响应函数
这样当事件被触发时，响应函数将会按照函数的绑定顺序执行
addEventListener()中的this是绑定事件的对象
不支持IE8及以下

attachEvent()
在IE8中及以下可以使用attachEvent()来绑定事件
参数：
1.事件的字符串，要on
2.回调函数
这个方法也可以同时为一个事件绑定多个处理函数
不同的是它是后绑定先执行，执行顺序和addEventListener()相反
attachEvent()中的this是window

# 清除事件的默认行为

DOM0:return false

DOM2:event.preventDefault


# 滚轮事件绑定的兼容性问题

## onmousewheel

鼠标滚轮滚动的事件，会在滚轮滚动时触发

但是火狐不支持该属性，在火狐中需要使用 DOMMouseScroll 来绑定滚动事件且该事件需要通过addEventListener()函数来绑定

## event.wheelDelta

可以获取鼠标滚轮滚动的方向
向上滚 120   向下滚 -120，wheelDelta这个值我们不看大小，只看正负就行了

wheelDelta这个属性火狐中不支持
在火狐中使用event.detail来获取滚动的方向
向上滚 -3  向下滚 3

