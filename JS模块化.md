# CommonJS

## 规范说明

每个文件都可当作一个模块
在服务器端：模块的加载是运行时同步加载的
在浏览器端：模块需要提前编译打包处理

## 基本语法

### 暴露模块

moudle.exports = {}
exports.xxx = xxx

### 引入模块

require(xxx)：第三方模块：xxx为模块名
自定义模块为路径

## 实现

### 服务器端实现：Node.js

### 浏览器端实现：Browserify

browserify js/src/app.js -o js/dist/bundle.js

# AMD(asynchronous module definition异步模块)

## 规范说明

专门用于浏览器端，模块的加载是异步的

## 基本语法

### 暴露模块

定义没有依赖的模块

```
define(function(){
    return 模块
})
```

定义有依赖的模块

```
define(["module1", "module2"], function(m1, m2) {
    return 模块
})
```

### 引入模块

```
require(["module1", "module2"],function(m1, m2) 
{
    
})
```

## 实现

### 浏览器端实现

Require.js

```
define(function () {
    let msg = "module1"

    function foo() {
        console.log(msg);
    }
    return {
        foo
    }
});
```

```
define([
    "module1"
], function (module1) {
    let msg = "module2"

    function foo() {
        module1.foo()
        console.log(msg)
    }
    return {
        foo
    }
});
```

```
(function () {
    requirejs.config({
        // baseUrl: "./js/modules",
        paths: {
            module1: "./js/modules/module1",
            module2: "./js/modules/module2"
        }
    })

    requirejs(["module2"], function (module2) {
        module2.foo()
    })
})()
```

```
<script data-main="./app.js" src="./js/libs/require.js"></script>
```

注意：jquery作为模块名要小写
angular要添加单独配置
```
shim: {
    angular: {
        exports: "angular"
    }
}
```

# ES6

## 规范说明

依赖模块需要编译打包处理

## 基本语法

### 导出模块

export {}

或export default {}

### 引入模块

import {对象的解构赋值} from "路径"

或import xxx from "路径"

引入第三方模块
import xxx from "包名"

## 实现

package.json

babel-cli(全局) babel-preset-es2015(--save-dev) browserify(全局)

定义.babelrc
{
    "presets":["es2015"]
}

使用babel将ES6编译为ES5
babel js/src -d js/lib

使用browserify编译js