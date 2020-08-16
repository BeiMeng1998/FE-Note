# 什么是Vue

渐进式JavaScript框架

# Vue的特点

1.遵循MVVM模式

2.编码简洁，体积小，运行效率高，适合移动/PC端开发

3.它本身只关注UI，可以轻松引入vue插件或其他第三方库开发项目

4.借鉴angular的模板和数据绑定技术

5.借鉴react的组件化和虚拟DOM技术

# 理解Vue的MVVM

![](_v_images/20200313222919315_21027.png =818x)

MVVM:

model：模型，数据对象（data）
view：视图，模板页面
viewmodel：视图模型（Vue的实例）

# 模板语法

## 模板的理解

动态的html页面
包含了一些JS语法代码
    双大括号表达式
    指令（以v-开头的自定义标签属性）

## 双大括号表达式

语法：{{exp}} 或 {{{exp}}}
功能：向页面输出数据
可以调用对象的方法

## 强制数据绑定

功能：指定变化的属性值

完整写法：
    v-bind: xxx="yyy" //yyy会作为表达式解析执行
   
简洁写法：
    :xxx="yyy"

## 绑定监听事件

功能：绑定指定事件名的回调函数

完整写法：
    v-on:click="xxx"

简洁写法：
    @click="xxx"

# 计算属性

在computed属性对象中定义计算属性的方法
初始化显示/相关属性发生改变时执行
在页面中使用{{方法名}}来显示计算的结果

# 计算属性高级

通过getter/setter实现对属性数据的显示和监视
计算属性存在缓存，多次读取只执行一次getter计算

# 监视属性

通过vm对象的$watch("属性名"，回调函数)方法或watch配置来监视指定的属性
当属性变化时，回调函数自动调用，在函数内部进行计算
```
watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
```

# 绑定class和style

## 理解

在应用界面中，某些元素的样式是变化的
class/style绑定就是专门来实现动态样式

## class绑定

:class = "xxx"
xxx是字符串/对象（属性名为类名，属性值为布尔值）/数组

## style绑定

:style="{color:activeColor, fontSize:fontSize + 'px'}"
其中activeColor/fontSize是data属性

# 条件渲染

v-if="xxx" v-else //通过移除标签显示隐藏
v-show="xxx" //通过样式显示隐藏

# 列表渲染

## 列表遍历显示

数组：v-for/index
对象：v-for/key

数组
```
<li v-for="(item, index) in persons" :key="index">
    {{index}} {{item.name}}
</li>
```
对象
```
<li v-for="(keyvalue, key) in persons[0]" :key="key">
    {{key}} {{keyvalue}}
</li>
```

## 列表的更新显示

删除item
替换item

# 事件处理

## 绑定监听

v-on:xxx="fun"
@xxx="fun"
@xxx="fun(参数)"
事件默认形参：event
隐含属性对象：$event

## 事件修饰符

@click.prevent="fun"相当于event.preventDefault()阻止事件的默认行为

@click.stop="fun" 相当于event.stopPropagation()阻止事件冒泡

## 按键修饰符

@keyup.keycode：操作的是某个keycode值的键
@keyup.enter：操作的是enter键

# Vue实例生命周期

![](_v_images/20200315140923612_16072.png =720x)

# 过渡&动画

## 理解

1.操作的是CSS的transition和animation

2.vue会给目标元素添加/移除特定的class

3.过渡的相关类名
.xxx-enter-active：指定显示的transition

.xxx-leave-active：指定隐藏的transition

xxx-enter/xxx-leave-to：指定隐藏时的样式

## 实现

1.给要执行过渡或动画的元素外包裹<transition name="xxx">

2.定义class样式
    过渡样式
    .xxx-enter-active：指定显示的transition
    .xxx-leave-active：指定隐藏的transition
    隐藏样式
    xxx-enter/xxx-leave-to：指定隐藏时的样式

CSS动画用法同CSS过渡，区别是在动画中v-enter类名在节点插入DOM后不会立即删除，而是在 animationend 事件触发时删除

# 过滤器

## 定义过滤器

```
Vue.filter(filterName, function(value[, arg1, arg2....]){
    进行一定额数据处理
    return newValue
})
```

## 使用过滤器

{{myData | filterName}}
{{myData | filterName(arg1...)}}

# 指令

内置指令与自定义指令

## 常用内置指令

### v:text

更新元素的text Content

### v-html

更新元素的inner HTML

### v-if v-else

如果为true，当前标签才会输出到界面

###  v-show

通过控制display样式来控制显示/隐藏

### v-for

遍历数组对象

### v-on

绑定事件监听，一般简写@

### v-bind

强制绑定解析表达式，可以省略加:

### v-model

双向数据绑定

### ref

指定唯一标识，vue对象通过$refs属性访问这个元素对象
this.$refs.xxx.innerHTML

### v-cloak

防止闪现，与CSS配合：[v-cloak]{display:none}

## 自定义指令

### 注册全局指令

```
Vue.directive("my-directive", function (el, binding) {//el：指令属性所在的标签对象，binding：包含指令相关信息数据的对象
    el.innerHTML = binding.value.toUpperCase()
})
```

### 注册局部指令

```
directives: {
    "my-directive": {
        bind (el, binding) {
            el.innerHTML =binding.value.toUpperCase()
        }
    }
}

```
### 使用指令

v-my-directive="xxx"

# 插件使用

Vue.use(插件名)//内部会执行 插件名.install(Vue)

# Vue-cli

vue-cli是vue官方提供的脚手架工具

## 创建vue项目

npm install -g vue-cli
vue init webpack vue_demo
cd vue_demo
npm install
npm run dev
访问：http://localhost:8080

## 基于脚手架编写项目

什么是组件?局部功能界面

### 子组件编写

```
<template>
    <div>
        <p class="msg">{{msg}}</p>
    </div>
</template>

<script>
export default {//配置对象（与Vue实例一致）
    name: "hello",
    data() {//必须写函数
        return {
            msg: "hello world"
        }
    },
}
</script>

<style>
    .msg {
        color: red;
        font-size: 40px;
    }
</style>
```

### 根组件编写

```
<template>
    <div>
        <img src="./assets/logo.png">
        <!--3.使用组件标签-->
        <hello/>
    </div>
</template>

<script>
//1.引入子组件
import hello from "./components/hello"

export default {
    name: "App",
    //2.映射组件标签
    components: {
        hello
    }
}
</script>

<style>

</style>
```

### 入口main.js编写

```
// 入口JS编写
// 1.引入Vue和根组件
import Vue from "vue" //注意vue是小写
import App from "./App"

new Vue({
    el: "#app",
    // 2.映射组件标签
    components: {
        APP
    },
    template:"<App />" // 3.根据Vue实例周期，若有template模板，将template编译到render渲染函数中
})
```

## 打包

npm run build

## 发布1：使用静态服务器工具包

npm install -g serve
serve dist
访问：localhost:5000

## 发布2：使用动态web服务器

修改配置：webpack.prod.conf.js
    output: {
        publicPath:"/xxx/" //打包文件夹名称
    }

重新打包：npm run build

修改dist文件夹为项目名称：xxx

将xxx拷贝到运行的 tomcat 的 webapps 目录下

访问：localhost:8080/xxx

# eslint

## 说明

1.eslint是一个代码规范检查工具
2.它定义了很多特定的规则，一旦你的代码违背了某一规则，eslint会做出非常有用的提示
3.基本已替代以前的jslint

## 修改.eslintrc.js的rule

规则错误三种等级：
0：关闭规则
1：打开规则，并且作为一个警告
2：打开规则，并且作为一个错误

# 组件化编码

1.拆分组件

2.静态组件

3.动态组件

# 组件间通信

## props

适用于父子间通信
:data = 'data'

props: ['data']
或
props: {
    data: {
        type: Number,
        required: true
    }
}

## 自定义事件

适用于父子组件通信
父组件定义事件并绑定到标签
@myEvent='myEvent'

子组件调用
this.$emit('myevent', data)

## 消息订阅与发布

适用于任意位置组件通信
npm i pubsub-js --save

向外暴露PubSub

两个方法

订阅消息：subscribe(callbackname, callback)--绑定监听
注意！callback函数第一个参数为callbackname，第二个参数为自定义参数

发布消息：publish(callbackname, variate)---触发回调

## slot插槽

组件间通信的内容不是数据或方法，而是标签

父组件给标签加 slot="slotName" 属性，相关方法或数据也应在父组件定义

子组件<slot name="slotName"></ slot>调用显示




# 存储数据

## localStorage读取数据

```
tasks: JSON.parse(window.localStorage.getItem('value') || '[]')
```

## localStorage+深度监视存储数据

```
watch: {
    tasks: {
        deep: true,
        handler: function (task) {
            window.localStorage.setItem('value', JSON.stringify(value))
        }
    }
}
```




