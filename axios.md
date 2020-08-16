# axios是什么？

1.前端最流行的ajax请求库
2.react/vue官方都推荐使用axios发ajax请求

# axios特点

1.基本promise的异步ajax请求库
2.浏览器端/node端都可以使用
3.支持请求/响应拦截器
4.支持请求取消
5.请求/响应数据转换
6.批量发送多个请求

# axios基本语法

axios(config): 通用/最本质的发任意类型请求的方式
axios(url[, config]): 可以只指定 url 发 get 请求
axios.request(config): 等同于 axios(config)
axios.get(url[, config]): 发 get 请求
axios.delete(url[, config]): 发 delete 请求
axios.post(url[, data, config]): 发 post 请求
axios.put(url[, data, config]): 发 put 请求

axios.defaults.xxx: 请求的默认全局配置
axios.interceptors.request.use(): 添加请求拦截器
axios.interceptors.response.use(): 添加响应拦截器

axios.create([config]): 创建一个新的 axios(它没有下面的功能)

axios.Cancel(): 用于创建取消请求的错误对象
axios.CancelToken(): 用于创建取消请求的 token 对象
axios.isCancel(): 是否是一个取消请求的错误
axios.all(promises): 用于批量执行多个异步请求
axios.spread(): 用来指定接收所有成功数据的回调函数的方法

# 难点语法的理解和使用


## axios.create([config])

1.根据指定配置创建一个新的axios instance, 每个新的axios instance都有自己的配置

2.新的axios instance没有取消请求和批量发送请求的方法

3.为什么要设计这个语法

例：两个后台一个端口号3000，一个端口号4000
```
axios.defaults.baseURL = 'http://localhost:3000'

axios.get('/get')

// 此时该如何在不修改全局配置的情况下，向端口4000发送请求？

const instance = axios.create({
    baseURL: 'http://localhost:4000'
})

instance.get('/get')
```

## 拦截器及运行流程

流程: 请求拦截器2 => 请求拦截器 1 => 发ajax请求 => 响应拦截器1 => 响
应拦截器 2 => 请求的回调

注意: 此流程是通过 promise 串连起来的, 请求拦截器传递的是 config, 响应
拦截器传递的是 response
```
axios.interceptors.request.use(
    config => {
        console.log('request interceptor1 onResolved')
        return config
    },
    error => {
        console.log('request interceptor1 onRejected')
        return Promise.reject(error)
    }
)

axios.interceptors.request.use(
    config => {
        console.log('request interceptor2 onResolved')
        return config
    },
    error => {
        console.log('request interceptor2 onRejected')
        return Promise.reject(error)
    }
)

axios.interceptors.response.use(
    response => {
        console.log('response interceptor1 onResolved')
        return response
    },
    error => {
        console.log('response interceptor1 onRejected')
        return Promise.reject(error)
    }
)

axios.interceptors.response.use(
    response => {
        console.log('response interceptor2 onResolved')
        return response
    },
    error => {
        console.log('response interceptor2 onRejected')
        return Promise.reject(error)
    }
)
axios.defaults.baseURL = 'http://localhost:3000'
axios.get('/get')
    .then(response => {
        console.log('response.data', response.data)
    })
    .catch(error => {
        console.log('error', error.message)
    })

```

## 取消请求

1. 基本流程
配置 cancelToken 对象
缓存用于取消请求的 cancel 函数
在后面特定时机调用 cancel 函数取消请求
在错误回调中判断如果 error 是 cancel, 做相应处理

2. 实现功能
点击按钮, 取消某个正在请求中的请求
在请求一个接口前, 取消前面一个未完成的请求

```
    let cancel  // 用于保存取消请求的函数

    // 添加请求拦截器
    axios.interceptors.request.use((config) => {
      // 在准备发请求前, 取消未完成的请求
      if (typeof cancel === 'function') {
          cancel('取消请求')
      }
      // 添加一个cancelToken的配置
      config.cancelToken = new axios.CancelToken((c) => { // c是用于取消当前请求的函数
        // 保存取消函数, 用于之后可能需要取消当前请求
        cancel = c
      })

      return config
    })

    // 添加响应拦截器
    axios.interceptors.response.use(
      response => {
        cancel = null
        return response
      },
      error => {
        if (axios.isCancel(error)) {// 取消请求的错误
          // cancel = null
          console.log('请求取消的错误', error.message) // 做相应处理
          // 中断promise链接
          return new Promise(() => {})
        } else { // 请求出错了
          cancel = null
          // 将错误向下传递
          // throw error
          return Promise.reject(error)
        }
      }
    )

    function getProducts1() {
      axios({
        url: 'http://localhost:4000/products1',
      }).then(
        response => {
          console.log('请求1成功了', response.data)
        },
        error => {// 只用处理请求失败的情况, 取消请求的错误的不用
          console.log('请求1失败了', error.message)
        }
      )
    }

    function getProducts2() {

      axios({
        url: 'http://localhost:4000/products2',
      }).then(
        response => {
          console.log('请求2成功了', response.data)
        },
        error => {
          console.log('请求2失败了', error.message)
        }
      )
    }

    function cancelReq() {
      // 执行取消请求的函数
      if (typeof cancel === 'function') {
        cancel('强制取消请求')
      } else {
        console.log('没有可取消的请求')
      }
    }
```

# axios源码分析

## axios 与 Axios 的关系

1.从语法上来说 axios 不是 Axios 的实例
2.从功能上来说 axios 是 Axios 的实例

如何理解?
通过源码可知
axios 是 Axios.prototype.request 函数通过 bind() 绑定 this 为 Axios实例 返回的新函数，注：bind() 返回的新函数内部是通过 call() 调用原函数
将 Axios 原型上的方法拷贝到 axios 上 ( request()/get()/post()/put()/delete()...
将 Axios 实例对象上的属性拷贝到 axios 上
所以 axios 作为对象有 Axios 原型对象上的所有方法, 有 Axios 对象上所有属性

## axios 与 axios.create  生成的 instance 的关系

axios.create(instanceconfig) 通过 mergeConfig(axios.defaults, instanceconfig) 生成一个可能与 axios 拥有不同默认配置的 instance
且源码在定义完axios.create()方法后
又单独给axios添加了 all 与 CancelToken等相关方法

所以：
二者相同点:
1.都是一个request(config)能发任意请求的函数
2.都有发特定请求的各种方法：get/post/put/delete
3.都有默认配置和拦截器属性：defaults/interceptors

二者不同点：
1.默认配置的值很可能不一样，因为mergeConfig的原因
2.instance 没有 axios后面添加的一些方法：create/CancelToken/all

## axios 运行整体流程

1.整体流程

request(config) ==> dispatchRequest(config) ==> xhrAdapter(config)

2.request(config)

将请求拦截器 / dispatchRequest() / 响应拦截器 通过 promise 链串连起来, 返回 promise

3.dispatchRequest(config)

转换请求数据 ===> 调用 xhrAdapter()发请求 ===> 请求返回后转换响应数
据. 返回 promise

4.xhrAdapter(config):

创建 XHR 对象, 根据 config 进行相应设置, 发送特定请求, 并接收响应数据, 返回 promise

### Promise 链式调用串联起 request(config) 流程

![](_v_images/20200521162202673_8874.png =717x)

chain数组默认为[diapatchRequest, undefined]

请求拦截器将fulfilled回调 (成功的回调传递的必须是config) 与rejected回调通过unshift方法加入到chain数组中，
而chain数组中回调的调用是每次都通过shift方法从数组最前面取出fulfilled回调与rejected回调，
这也就导致了先定义的请求拦截器后执行

响应拦截器则是将fulfilled回调与rejected回调push到chain数组中，所以是先定义先执行

而我们定义的发送请求的then回调会被加到chain数组的末尾，最后调用

### dispatchRequest(config)

dispatchRequest(config) 本身不会发送请求，它会在自身调用 xhrAdapter(config) 发送请求

dispatchRequest做的是转换请求数据以及响应数据

dispatchRequest 函数本身做请求体数据转换以及设置默认请求头的工作，如默认将 data 中的对象转换为 JSON数据

而对于响应数据，它将响应的JSON数据解析，若响应非JSON数据，则会通过try/catch忽略，不会做任何处理

### xhrAdapter(config)

AJAX封装

reponse {
    data,
    status,
    statusText,
    headers,
    config,
    request
}

error {
    message,
    request,
    response
}

### 如何中断请求

1. 当配置了 cancelToken 对象时, 保存 cancel 函数
(1) 创建一个用于将来中断请求的 cancelPromise
(2) 并定义了一个用于取消请求的 cancel 函数
(3) 将 cancel 函数传递出来

2. 调用 cancel()取消请求
(1) 执行 cacel 函数, 传入错误信息 message
(2) 内部会让 cancelPromise 变为成功, 且成功的值为一个 Cancel 对象
(3) 在 cancelPromise 的成功回调中中断请求, 并让发请求的 proimse 失败, 失败的 reason 为 Cacel 对象