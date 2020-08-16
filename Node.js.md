# 命令行窗口

dir cd md rd

环境变量（windows系统中的变量）

当我们在命令行窗口打开一个文件或调用一个程序时
系统首先会在当前目录下寻找文件程序，如果找到则直接打开
如果没找到会依次到环境变量的path的路径中寻找

# 进程和线程

进程负责为程序运行提供必备的环境，进程就相当于工厂中的车间

线程是计算机中的最小的计算单位，线程负责执行进程中的程序，线程就相当于工厂中的工人

# 什么是Node.js

Node.js是一个能够在服务器端运行JavaScript的JavaScript运行环境

Node采用Google开发的V8引擎运行js代码，使用事件驱动，非阻塞和异步I/O模型等技术来提高性能
可优化应用程序的传入量和规模

传统的服务器都是多线程的
每进来一个请求，就创建一个线程去处理请求

Node的服务器是单线程的
Node处理请求时是单线程，但是在后台拥有一个I/O线程池

# COMMONJS规范

## ECMAScript5标准的缺陷

1.没有模块系统

什么是模块化？类似于红白机的卡带
模块化的好处，降低耦合性，方便代码复用
达到高内聚低耦合的目的，降低开发成本
如果程序设计的规模达到了一定程度，则必须对其进行模块化
模块化可以有多种形式，但至少应该提供将代码分割为多个源文件的机制
COMMONJS的模块功能可以帮我们解决该问题

2.标准库较少

3.没有标准接口

4.缺乏管理系统

## COMMONJS规范

COMMONJS规范的提出，主要是为了弥补JavaScript没有标准的缺陷

为JS指定了一个美好的愿景，希望JS能够在任何地方运行

COMMONJS对模块的定义十分简单
1.模块引用
2.模块定义
3.模块标识

# Node模块化

在Node中，一个JS文件就是一个模块，都是独立运行在一个函数中，而不是全局作用域，所以一个模块中的变量和函数在其他模块中无法访问

向外暴露模块的属性或方法，将属性和方法设置给exports的属性
exports.x = "我是该模块暴露的x"

在Node中，通过require（）函数来引入外部的模块

require（）可以传递一个文件的路径作为参数，Node将会自动根据该路径来引入外部模块
注意：这里的路径如果使用相对路径，必须以 . 或 .. 开头

使用require（）引入模块以后，该函数会返回一个对象，这个对象代表的是引入的模块

我们使用require引入外部模块时，传入的参数就是模块标识，我们可以通过模块标识来找到指定的模块
模块分为两大类：
1.核心模块：由node引擎提供的模块，直接用模块名就能引用

2.文件模块：用户自己创建的模块，文件模块的标识就是文件的路径（绝对路径，相对路径）

在node中有一个全局对象global，它的作用和网页中的window类似：
在全局中创建的变量会作为global的属性保存，
在全局中创建的函数会作为global的方法保存

# Node模块化详解

```
function (exports, require, module, __filename, __dirname) {
console.log(arguments.callee + "")
}

```
实际上模块中的代码都是包装在一个函数中执行得，并且在执行时传递进了5个实参

## exports

该对象将变量或函数暴露到外部

## require

函数，用来引入外部的模块
注意优先从缓存加载，避免重复加载，提高模块加载效率
require('模块标识')

## module

module代表的是当前模块本身
exports就是module的属性
也就是说既可以使用 exports导出，也可以使用module.exports导出

exports 和 module.exports的区别
通过exports只是module.exports对象的引用
而最终return的是module.exports
也就是说给exports重新赋值对象地址不会影响module.exports

## __filename

当前模块的完整路径

## __dirname

当前模块所在目录的完整路径

# Package

COMMONJS的包规范允许我们将一组相关的模块组合到一起，形成一组完整的工具
COMMONJS的包规范由 包结构 和 包描述文件 两部分组成

包结构：用于组织包中的各种文件
包描述文件：描述包的相关信息，以供外部读取分析

package.json：描述文件
bin：可执行二进制文件
lib：js代码库
doc：文档
test：单元测试

包搜索过程，先是同级目录下node_modules目录
找包
找包的package.json
找json中的main属性
main记录了包的入口模块
没有main则默认index.js

找不到向上级查找，一直找到磁盘根目录，没有则报错

# package-lock.json

1.保存node_modules中所有包的信息，这样npm install下载依赖时会提升速度
2.锁定版本，防止自动升级新版

# 什么是NPM

(Node Package Manager)

COMMONJS包规范是理论，NPM就是其中的一种实践

对于Node而言，NPM帮助完成其第三方模块的发布，安装和依赖等。借助NPM,Node与第三方模块之间形成了很好的生态系统

## NPM命令

npm -v :查看版本

npm --help: 帮助说明

npm install --global npm :升级npm

npm init -y :创建package.json

npm search 包名 : 搜索模块包

npm install : 下载当前项目的依赖包

npm install 包名 : 在当前目录安装包

npm install 包名 --save : 安装包并添加到依赖中

npm install 包名 -g : 全局模式安装包

npm install 文件路径 : 从本地安装

npm config set registry 地址 : 设置镜像源

npm install 包名 -registry=地址 : 从镜像源安装

npm remove 包名 : 删除一个包

# Node搜索包的流程

Node在使用模块名字来引入模块时，它首先在当前目录的node_modules中寻找是否有该模块
有则使用，无则逐级向上层目录node_modules中寻找
直到找到磁盘根目录，如果依然没有，则报错

# BUFFER缓冲区

Buffer的数据结构和数组很像，操作的方法也和数组类似
JS数组的缺陷：数组不能存储二进制文件，而Buffer就是专门用来存储二进制数据
使用Buffer不需要引入模块，直接使用即可
在Buffer中存储的都是二进制数据，但是在显示时以16进制显示
Buffer每一个元素大小为8bit 1byte
Buffer.length表示的是Buffer所占用的字节，占用内存的大小

创建一个指定大小的Buffer
注意：Buffer的所有构造函数都是不推荐使用的

例：创建一个连续的10字节大小的Buffer
`var buf = Buffer.alloc(10);`同时清空这10字节内存中的数据

`var buf = Buffer.allocUnsafe(10)`不清空内存，可能泄露敏感数据，但性能比alloc高

注意：
1.Buffer的大小一旦确定就无法修改，Buffer是对底层内存的直接操作
2.Buffer每个元素大小为 8bit 1byte 高位截取舍弃
3.数字在控制台或页面中输出一定是10进制，想要进制变换只能.toString(16)

例：将一个字符串转换为Buffer
`var buf = Buffer.from(str)`

例：将Buffer里的二进制数据转换为字符串
`buf.toString()`

# fs（文件系统）

在Node中与文件系统的交互式非常重要的，服务器的本质就是将本地的文件发送给远程的客户端

Node通过fs模块来和文件系统进行交互

该模块提供了一些标准文件访问API来打开，读取，写入文件，以及与其交互

要使用fs模块，首先需要对其进行加载
fs是核心模块，直接引入不需要下载

文件读写的相对路径是相对于执行node命令所在的目录决定的

## 同步和异步调用

fs模块中所有的操作都有两种形式可供选择：同步和异步

同步文件系统会阻塞程序的执行，也就是除非操作完毕，否则不会向下执行代码

异步文件系统不会阻塞程序的执行，而是在操作完成时，通过回调函数将结果返回

## 同步文件写入

### 打开文件

`fs.openSync(path, flags[, mode])`
参数
path:要打开文件的路径
flags:rwx
mode:设置文件的操作权限

该方法会返回一个文件的描述符fd作为结果，我们可以通过该描述符来对文件进行各种操作

### 向文件中写入内容

`fs.writeSync(fd, string[, position[, encoding]])`
参数
fd:文件的描述符
string:要写入的内容
position:开始写入的位置
encoding:编码

### 保存并关闭

`fs.closeSync(fd)`

## 异步文件写入

打开文件
`fs.open(path, flags[, mode], callback)`
异步调用的方法，结果都是通过回调函数返回的
回调函数的两个参数：
err：错误对象，没有则为null
fd：文件的描述符号

写入文件
`fs.write(fd, string[, position[, encoding]], callback)`
回调会接收到参数 (err, written, string)，其中 written 指定传入的字符串中被要求写入的字节数。 被写入的字节数不一定与被写入的字符串字符数相同。

关闭文件
`fs.close(fd, callback)`
回调参数除了可能的异常，完成回调没有其他参数。

例：
```
fs.open("hello.txt", "w", function (err, fd) {
    if (err) {
        console.log(err)
    } else {
        fs.write(fd, "异步写入", function (err) {
            if (err) {
                console.log("写入失败")
            } else {
                console.log("写入成功")
            }
        })
        fs.close(fd, function(err) {
            if (err) {
                console.log("关闭失败")
            } else {
                console.log("关闭成功")
            }
        })
    }
})
```

## 简单文件写入

`fs.writeFileSync(file, data[, options])`
`fs.writeFile(file, data[, options], callback)`
参数
file：操作的文件路径
data：要写入的数据
options：选项，可以对写入进行一些设置
{
    encoding <string> | <null> 默认值: 'utf8'。
    mode <integer> 默认值: 0o666。
    flag <string> 参阅支持的文件系统标志。默认值: 'w'。
}
![](_v_images/20200306165231322_8599.png)
callback：当写入完成执行的函数，回调参数err

## 流式文件写入

同步，异步，简单文件的写入都不适合大文件的写入，性能较差，容易导致内存泄漏


创建一个可写流
`fs.createWriteStream(path[, options])`
参数
path:文件路径
options：配置的参数
例：
`var ws = fs.createWriteStream("hello.txt")`

流式文件写入
`ws.write("通过可写流写入的内容")`

可以通过监听流的open和close事件来监听流的打开和关闭，均为一次性事件
```
ws.once("open", function () {
    console.log("流已打开")
})
```

关闭流
`ws.close()`相当于切断接收端的管道
`ws.end()`想打关于切断发送端的管道

## 简单文件读取

同步文件，异步文件读取同写入

`fs.readFile(path[, options], callback)`
`fs.readFileSync(path[, options])`
参数：
path：要读取的文件的路径
options：读取的选项
callback回调函数两个参数（err：错误对象, data：读取到的数据，会返回一个buffer）
例：
```
fs.readFile("hello.txt", function(err, data) {
    if (!err) {
        console.log(data.toString())
    }
})
```

## 流式文件读取

同步，异步，简单文件的读取都不适合大文件的写入，性能较差，容易导致内存泄漏
流式文件读取也适用于一些比较大的文件，可以分多次将文件读取到内存中

创建一个可读流
`var rs = fs.createReadStream("01.jpg")`
可读流开启不进行操作不会关闭

如果要读取一个可读流中的数据，必须要为可读流绑定一个data事件，data事件绑定完毕会自动开始读取数据，每次读取65536字节，读取完关闭可读流
```
rs.on("data", function (data) {
    console.log(data)
})
```

若想将可读流写入可写流直接用可读流的pipe()方法，完成自动关闭可读流和可写流
`rs.pipe(ws)`

## fs模块的其他方法

验证路径文件是否存在
`fs.existsSync(path)`
返回true或false

获取文件状态
`fs.stat(path, callback)`
`fs.statSync(path)`
回调函数返回一个err对象和stats对象，stats对象里保存了当前文件的相关状态
stats相关方法
isFile() isDirectory()

删除文件
`fs.unlink(path, callback)`
`fs.unlinkSync(path)`

读取目录结构
`fs.readdir(path[, options], callback)`
`fs.readdirSync(path[, options])`
回调函数两个参数err和files（是一个字符串数组，数组元素为目录下文件和目录名）

截断文件
`fs.truncateSync(path, size)`

创建目录
`fs.mkdir(path[, mode], callback)`
`fs.mkdirSync(path[, mode])`

删除目录
`fs.rmdir(path, callback)`
fs.rmdirSync(path)

重命名与剪切
`fs.rename(oldpath, newpath, callback)`
`fs.renameSync(oldpath, newpath)`
回调函数参数err

监视文件的修改
`fs.watchFile(filename[, options], listener)`
参数：
filename：要监视的文件的名字
options：配置选项
listener：回调函数，文件发生变化时，回调函数会执行
回调函数中有两个参数curr和prev
curr 当前文件的状态
prev 修改前文件的状态
这两个都是stats对象

# http

IP地址用来定位联网的计算机，端口号用来定位具体的应用程序
服务器发送的文本数据默认utf-8编码
而浏览器默认按照系统默认编码gbk去解析
解决方法`response.setHeader('Content-Type', 'text/plain; charset=utf-8')`
text/plain:普通文本
text/html:html文本
不同的资源Content-Type不同，图片不需要编码

```
// 加载http核心模块
const http = require('http')

// 创建一个Web服务器，返回server实例
const server = new http.createServer()

// 服务器收到请求，提供服务
// 回调接收两个参数
// request 请求对象 请求对象可以用来获取客户端的一些信息
// response 响应对象 响应对象可以给客户端发送响应信息
server.on('request', (request, response) => {
    console.log(request.url)

    // response.write方法给客户端发送响应数据
    // write方法可以使用多次，但一定要end来结束响应，否则客户端会一直等待，一般不用
    // response.write('hello wrold')
    // response.end()

    // 也可以通过end发送响应数据同时结束响应
    // 响应数据只能是字符串或者二进制数据
    let data = [
        {
            name: 'a',
            age: 18
        },
        {
            name: 'b',
            age: 14
        },{
            name: 'c',
            age: 18
        },
    ]
    response.setHeader('Content-Type', 'text/plain; charset=utf-8')
    response.end(JSON.stringify(data))
})

// 绑定端口号，启动服务器
server.listen(3000,() => {
    console.log('服务器启动，端口号:3000')
})
```

# path

path.basename()

path.dirname()

path.extname()

path.parse()

path.join()

path.isAbsolute()

path.resolve()

# 服务端重定向只针对同步请求才有效