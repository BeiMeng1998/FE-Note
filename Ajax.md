# Ajax

不用刷新整个页面便可与服务器通信的技术，用于局部更新数据

![](_v_images/20200421195557141_9361.png =750x)

# Ajax实现步骤

1.创建Ajax对象

`let xhr = new XMLHttpRequest()`

2.定义请求地址以及请求方式

`xhr.open('get', 'http://127.0.0.1:3000')`

3.发送请求

`xhr.send()`

4.获取服务器端的响应数据

`xhr.onload = () => { console.log(xhr.resposeText) }`

# 服务器响应的数据格式

字符串与二进制

# 请求参数传递

GET请求方式

`xhr.open('get', 'http://127.0.0.1/?name=zhangsan&age=20')`

POST请求方式

设置请求报文首部
`xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')`
在send中传参
`xhr.send('name=zhangsan&age=20')`

# 请求参数格式

1.application/x-www-form-urlencoded

`'name=zhangsan&age=20'`

2.application/json

`'{"name": "zhangsan", "age": 20}'`

# Ajax状态码

0：请求未初始化，未调用open
1：请求已经建立，还未发送，未调用send
2：请求已经发送
3：请求正在处理中，通常响应中已经有部分数据可以使用了
4：请求已经完成，可以获取并使用服务器响应数据

## 获取Ajax状态码

xhr.readyState

onreadystatechange
当Ajax状态码发生改变时触发该事件

# 获取服务器响应的区别

onload事件不兼容IE低版本，只触发一次，无需判断Ajax状态码
onreadystatechange事件兼容IE低版本，触发多次，需要判断Ajax状态码

# 获取响应状态码

xhr.status

# 获取响应报文首部

xhr.getResponseHeader()

获取响应报文数据类型

xhr.getResponseHeader('Content-Type')

# 低版本IE浏览器优先从缓存加载问题

在请求地址不发生改变的情况下，只有第一次请求会发送到服务端
后续请求会从浏览器缓存中获取结果
同时低版本IE也不支持onload事件

解决方案：

`xhr.open('get', `http://127.0.0.1:3000/?t=${Math.random()}`)`

# 客户端使用art-template

1.下载template并引入

`<script src="./js/template-web.js"></script>`

2.创建一个art-template模板

```
<h1 id='container'>hello world</h1>
<script id="tp1" type="text/html">
    {{ name }} {{ age }}
</script>
```

3.模板数据赋值获得html字符串

`const html = template('tpl', {name: '张三', age: 20})`

4.将处理好的html字符串添加到页面中

` document.getElementById('container').innerHTML = html`

# 节流函数

```
let timer = null
clearTimeout(timer)
timer = setTimeout(() => {}, 500)
```

# FormData对象

## FormData对象的作用

### 1.将HTML表单映射成表单对象

客户端

```
    <form id='form'>
        <input type="text" name="username">
        <input type="password" name="password">
        <input type="button" value="提交" id='btn'>
    </form>

    <script>
        const btn = document.getElementById('btn')
        btn.onclick = () => {
            const form = document.getElementById('form')
            const formData = new FormData(form)
            const xhr = new XMLHttpRequest()
            xhr.open('post', 'http://127.0.0.1:3000/post')
            xhr.send(formData)
            xhr.onload = () => {
                console.log(xhr.responseText)
            }
        }
    </script>
```

服务端

npm i formidable
```
const formidable = require('formidable')
app.post('/post', (req, res) => {
    const form = new formidable.IncomingForm()
    form.parse(req, (err, fields, files) => {
        res.send(fields)
    })
})
```

### 2.异步上传二进制文件

客户端

```
    <label for="file">请选择文件</label>
    <input type="file" id="file" name='file'>
    <script>
        const file = document.getElementById('file')       
        file.onchange = () => {
            const formData = new FormData()  
            formData.append('attrName', file.files[0])
            const xhr = new XMLHttpRequest()
            xhr.open('post', 'http://127.0.0.1:3000/post')
            xhr.send(formData)
            xhr.onload = () => {
                if (xhr.status === 200) {
                    console.log(xhr.responseText)
                }
            }
        }
    </script>
```

服务端

```
app.post('/post', (req, res) => {
    const form = new formidable.IncomingForm()
    form.uploadDir = './public/uploads'
    form.keepExtensions = true
    form.parse(req, (err, fields, files) => {
        res.send('OK')
    })
})
```

上传文件进度

```
xhr.upload.onprogress = ev => {
    ev.loaded / ev.total
}
```
获取上传资源图片地址

```
app.post('/post', (req, res) => {
    const form = new formidable.IncomingForm()
    form.uploadDir = './public/uploads'
    form.keepExtensions = true
    form.parse(req, (err, fields, files) => {
        res.send({
            path: '/public' + files.attrName.path.split('public')[1]
        })
    })
})
```

## FormData对象实例方法

### get('key')

获取表单对象的属性值

### set('key', value)

设置表单对象的属性值
set会覆盖相同属性名

### delete('key')

删除表单属性值

### append('key', value)

添加表单对象属性
append不会覆盖相同属性名，保留两个

# AJAX请求限制

## 同源政策

同源政策的目的，是为了保证用户信息的安全，防止恶意的网站窃取数据。

三个相同：
1.协议相同
2.域名相同
3.端口相同

限制范围
1.无法读取非同源网页的Cookie，LocalStorage, IndexedDB
2.无法接触非同源网页的DOM
3.无法向非同源地址发送Ajax请求，（可以发送，但浏览器会拒绝接受响应）

# JSONP解决跨域问题

JSONP是json with padding的缩写
它不属于Ajax请求，但它可以模拟Ajax请求
利用script标签可以向非同源地址发送请求

1.在客户端全局作用域下定义函数fn

`function fn (data) {}`

2.在fn函数内部对服务器端返回的数据进行处理

`function fn (data) {console.log(data)}`

3.将不同源的服务器请求地址写在script标签的src属性中

`<script src="http://127.0.0.1:3001/get?callback=fn"></script>`


4.服务器端响应数据必须是一个函数的调用，真正要发送给客户端的数据需要作为函数调用的参数

```
const data = req.query.callback + '({name: '张三',age: 18})'
res.send(data)
```

## JSONP封装

客户端

```
<body>
    <button id="btn">点我</button>
    <script>
        const btn = document.getElementById('btn')
        btn.onclick = () => {
            jsonp({
                url: 'http://127.0.0.1:3001/jsonp',
                data: {
                    name: '张三',
                    age: 20
                },
                success(data) {
                    console.log(data)
                }
            })
        }
        function jsonp(options) {
            const fnName = 'jsonp' + Math.random().toString().replace('.', '')
            window[fnName] = options.success

            let param = ''
            for (const key in options.data) {
                if (options.data.hasOwnProperty(key)) {
                    param += `&${key}=${options.data[key]}`
                }
            }

            const scriptNode = document.createElement('script')
            scriptNode.src = `${options.url}?callback=${fnName}${param}`
            document.body.appendChild(scriptNode)
            scriptNode.onload = () => {
                document.body.removeChild(scriptNode)
            }
        }
    </script>
</body>
```

服务端

```
app.get('/jsonp', (req, res) => {
    // const fnName = req.query.callback   
    // const data = JSON.stringify(req.query)
    // console.log(data);
    // res.send(`${fnName}(${data})`)
    res.jsonp(req.query)
})
```

# CROS跨域资源共享

CROS全称：cross-origin resource sharing
它允许浏览器向跨域服务器发送Ajax请求，客服了Ajax只能同源使用的限制

跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站通过浏览器有权限访问哪些资源。另外，规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求），浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨域请求。服务器确认允许之后，才发起实际的 HTTP 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证相关数据）。

![](_v_images/20200424135053073_23492.png =797x)

服务端

```
app.use((req, res, next) => {
    res.header('Access-Control-Allow-Origin', '*')
    res.header('Access-Control-Allow-Methods', '*')
    next()
})
```

# 服务器端解决跨域问题

服务器端不存在同源政策限制

![](_v_images/20200424143301343_16747.png =688x)

npm i request

```
const request = require('request');
request('http://www.google.com', function (error, response, body) {
  console.error('error:', error); // Print the error if one occurred
  console.log('statusCode:', response && response.statusCode); // Print the response status code if a response was received
  console.log('body:', body); // Print the HTML for the Google homepage.
});
```

# 携带Cookie跨域登录

在使用Ajax技术发送跨域请求时，默认情况下不会在请求中携带cookie信息

## withCredentials = true

指定在涉及跨域请求时，是否携带cookie信息，默认值为false

## Access-Control-Allow-Credentials : true

允许客户端发送请求时携带cookie

# $.ajax()

```
$.ajax({
    type: 'get',
    url: 'http://www.example.com',
    data: {name: '张三', age: 20},
    contentType: 'application/x-www-form-urlencoded',
    beforeSend() {},
    success(response){},
    error(xhr){}
})
```

## serialize方法

将表单中的数据自动拼接成字符串类型参数

`const param = $('form').serialize()`

## $.ajax()发送jsonp

```
$.ajax({
    url: 'http://www.example.com',
    dataType: 'jsonp',
    // jsonp: 'callback',
    // jsonCallback: 'fnName',
    success(response) {
    }
})
```

# $.get(), $.post()

`$.get('www.example.com', {name:'张三'}, (res) => {})`

# jQuery中Ajax全局事件

ajaxStart
ajaxComplete