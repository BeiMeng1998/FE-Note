# express

# express下配置art-template

npm i art-template
npm i express-art-template
app.engine('html', require('express-art-template'))

Express为Response对象提供了render方法
默认不可使用，在配置了模板引擎后可用

res.render('html模板名', {模板数据})
第一个参数不写路径，默认到views目录中查找该模板文件

如果希望修改默认的views视图渲染目录
app.set('views', '目录路径')

# 静态资源

app.use(express.static('public'));

app.use('/static', express.static('public'));

app.use('/static', express.static(__dirname + '/public'));

# GET与POST

GET 用于获取信息，是无副作用的，是幂等的，且可缓存
POST 用于修改服务器上的数据，有副作用，非幂等，不可缓存

GET 和 POST 方法没有实质区别，只是报文格式不同。
GET 方法的参数应该放在 URL 中，POST 方法参数应该放在 Body 中

从传输的角度来说，他们都是不安全的，因为 HTTP 在网络上是明文传输的，只要在网络节点上捉包，就能完整地获取数据报文。要想安全传输，就只有加密，也就是 HTTPS。

HTTP 协议没有 Body 和 URL 的长度限制，对 URL 限制的大多是浏览器和服务器的原因。

## 在express中如何获取GET请求数据

res.query能拿到GET请求参数

## 在express中如何获取POST请求数据

express中没有内置获取POST请求体的API
使用中间件body-parser

```
var express = require('express')
var bodyParser = require('body-parser')

var app = express()

app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())

app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  res.end(JSON.stringify(req.body, null, 2))
})
```
req.body得到请求数据

# app.js

```
const express = require('express')
const path = require('path')
const bodyParser = require('body-parser')
const router = require('./router')
const app = express()

app.engine('html', require('express-art-template'))
app.use('/public/', express.static(__dirname, './public/'))
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())

app.use(router)

app.listen(3000, () => {
    console.log('listening on port 3000')
})
```

# router.js

```
const express = require('express')
const fs = require('fs')

const router = express.Router()
router.get('/', (req, res) => {
    res.render('index.html')
})
module.exports = router
```

# express-session

express默认不支持cookie和session

npm i express-session
```
const session = require('express-session')

app.use(session({
  secret: 'keyboard cat',
  resave: false,
  saveUninitialized: false
}))

req.session.name = ???
console.log(req.session.name)
```
