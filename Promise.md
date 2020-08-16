# Promise深入 + 自定义Promise
## 1. 准备
### 1.1. 函数对象与实例对象
    1. 函数对象: 将函数作为对象使用时, 简称为函数对象
    2. 实例对象: new 函数产生的对象, 简称为对象

### 1.2. 回调函数的分类
    1. 同步回调: 
        理解: 立即执行, 完全执行完了才结束, 不会放入回调队列中
        例子: 数组遍历相关的回调函数 / Promise的excutor函数
    2. 异步回调: 
        理解: 不会立即执行, 会放入回调队列中将来执行
        例子: 定时器回调 / ajax回调 / Promise的成功|失败的回调

### 1.3. JS中的Error
    1. 错误的类型
        Error: 所有错误的父类型
        ReferenceError: 引用的变量不存在
        TypeError: 数据类型不正确的错误
        RangeError: 数据值不在其所允许的范围内
        SyntaxError: 语法错误
    2. 错误处理
        捕获错误: try ... catch
        抛出错误: throw error
    3. 错误对象
        message属性: 错误相关信息
        stack属性: 函数调用栈记录信息

## 2. Promise的理解和使用
### 2.1. Promise是什么?
    1.抽象表达: 
        Promise是JS中进行异步编程的新的解决方案(旧的是谁?)
    2.具体表达:
        从语法上来说: Promise是一个构造函数
        从功能上来说: promise对象用来封装一个异步操作并可以获取其结果
    3. promise的状态改变(只有2种, 只能改变一次)
        pending变为resolved
        pending变为rejected
    4. promise的基本流程
![promise基本流程](http://vipkshttp1.wiz.cn/ks/share/resources/49c30824-dcdf-4bd0-af2a-708f490b44a1/92b8cbfb-a474-4859-943b-6048e9dc66f6/index_files/9b2b980e2959c4f996cafddb03fa5d4d.png)

### 2.2. 为什么要用Promise?
    1. 指定回调函数的方式更加灵活: 可以在请求发出甚至结束后指定回调函数
    2. 支持链式调用, 可以解决回调地狱问题

### 2.3. 如何使用Promise?
    1. 主要API
        Promise构造函数: Promise (excutor) {}
        Promise.prototype.then方法: (onResolved, onRejected) => {}
        Promise.prototype.catch方法: (onRejected) => {}
        Promise.resolve方法: (value) => {}
        Promise.reject方法: (reason) => {}
        Promise.all方法: (promises) => {}
        Promise.race方法: (promises) => {}
    2. 几个重要问题
        如何改变promise的状态?
        一个promise指定多个成功/失败回调函数, 都会调用吗?
        promise.then()返回的新promise的结果状态由什么决定?
        改变promise状态和指定回调函数谁先谁后?
        promise如何串连多个操作任务?
        promise异常传(穿)透?
        中断promise链

## 3. 自定义Promise
    1. 定义整体结构
    2. Promise构造函数的实现
    3. promise.then()/catch()的实现
    4. Promise.resolve()/reject()的实现
    5. Promise.all/race()的实现
    6. Promise.resolveDelay()/rejectDelay()的实现
    7. ES6 class版本

```
/* 
自定义Promise函数模块: IIFE
*/
(function (window) {

  const PENDING = 'pending'
  const RESOLVED = 'resolved'
  const REJECTED = 'rejected'

  class Promise {
    /* 
    Promise构造函数
    excutor: 执行器函数(同步执行)
    */
    constructor(excutor) {
      // 将当前promise对象保存起来
      const self = this
      self.status = PENDING // 给promise对象指定status属性, 初始值为pending
      self.data = undefined // 给promise对象指定一个用于存储结果数据的属性
      self.callbacks = [] // 每个元素的结构: { onResolved() {}, onRejected() {}}

      function resolve(value) {
        // 如果当前状态不是pending, 直接结束
        if (self.status !== PENDING) {
          return
        }

        // 将状态改为resolved
        self.status = RESOLVED
        // 保存value数据
        self.data = value
        // 如果有待执行callback函数, 立即异步执行回调函数onResolved
        if (self.callbacks.length > 0) {
          setTimeout(() => { // 放入队列中执行所有成功的回调
            self.callbacks.forEach(calbacksObj => {
              calbacksObj.onResolved(value)
            })
          });
        }

      }

      function reject(reason) {
        // 如果当前状态不是pending, 直接结束
        if (self.status !== PENDING) {
          return
        }

        // 将状态改为rejected
        self.status = REJECTED
        // 保存value数据
        self.data = reason
        // 如果有待执行callback函数, 立即异步执行回调函数onRejected
        if (self.callbacks.length > 0) {
          setTimeout(() => { // 放入队列中执行所有成功的回调
            self.callbacks.forEach(calbacksObj => {
              calbacksObj.onRejected(reason)
            })
          });
        }
      }

      // 立即同步执行excutor
      try {
        excutor(resolve, reject)
      } catch (error) { // 如果执行器抛出异常, promise对象变为rejected状态
        reject(error)
      }

    }

    /* 
    Promise原型对象的then()
    指定成功和失败的回调函数
    返回一个新的promise对象
    返回promise的结果由onResolved/onRejected执行结果决定

    */
    then(onResolved, onRejected) {
      const self = this

      // 指定回调函数的默认值(必须是函数)
      onResolved = typeof onResolved === 'function' ? onResolved : value => value
      onRejected = typeof onRejected === 'function' ? onRejected : reason => {
        throw reason
      }


      // 返回一个新的promise
      return new Promise((resolve, reject) => {

        /* 
        执行指定的回调函数
        根据执行的结果改变return的promise的状态/数据
        */
        function handle(callback) {
          /* 
          返回promise的结果由onResolved/onRejected执行结果决定
          1. 抛出异常, 返回promise的结果为失败, reason为异常
          2. 返回的是promise, 返回promise的结果就是这个结果
          3. 返回的不是promise, 返回promise为成功, value就是返回值
          */
          try {
            const result = callback(self.data)
            if (result instanceof Promise) { // 2. 返回的是promise, 返回promise的结果就是这个结果
              /* 
              result.then(
                value => resolve(vlaue),
                reason => reject(reason)
              ) */
              result.then(resolve, reject)
            } else { // 3. 返回的不是promise, 返回promise为成功, value就是返回值
              resolve(result)
            }

          } catch (error) { // 1. 抛出异常, 返回promise的结果为失败, reason为异常
            reject(error)
          }
        }

        // 当前promise的状态是resolved
        if (self.status === RESOLVED) {
          // 立即异步执行成功的回调函数
          setTimeout(() => {
            handle(onResolved)
          })
        } else if (self.status === REJECTED) { // 当前promise的状态是rejected
          // 立即异步执行失败的回调函数
          setTimeout(() => {
            handle(onRejected)
          })
        } else { // 当前promise的状态是pending
          // 将成功和失败的回调函数保存callbacks容器中缓存起来
          self.callbacks.push({
            onResolved(value) {
              handle(onResolved)
            },
            onRejected(reason) {
              handle(onRejected)
            }
          })
        }
      })
    }

    /* 
    Promise原型对象的catch()
    指定失败的回调函数
    返回一个新的promise对象
    */
    catch (onRejected) {
      return this.then(undefined, onRejected)
    }

    /* 
    Promise函数对象的resolve方法
    返回一个指定结果的成功的promise
    */
    static resolve = function (value) {
      // 返回一个成功/失败的promise
      return new Promise((resolve, reject) => {
        // value是promise
        if (value instanceof Promise) { // 使用value的结果作为promise的结果
          value.then(resolve, reject)
        } else { // value不是promise  => promise变为成功, 数据是value
          resolve(value)
        }
      })
    }

    /* 
    Promise函数对象的reject方法
    返回一个指定reason的失败的promise
    */
    static reject = function (reason) {
      // 返回一个失败的promise
      return new Promise((resolve, reject) => {
        reject(reason)
      })
    }

    /* 
    Promise函数对象的all方法
    返回一个promise, 只有当所有proimse都成功时才成功, 否则只要有一个失败的就失败
    */
    static all = function (promises) {
      // 用来保存所有成功value的数组
      const values = new Array(promises.length)
      // 用来保存成功promise的数量
      let resolvedCount = 0
      // 返回一个新的promise
      return new Promise((resolve, reject) => {
        // 遍历promises获取每个promise的结果
        promises.forEach((p, index) => {
          Promise.resolve(p).then(
            value => {
              resolvedCount++ // 成功的数量加1
              // p成功, 将成功的vlaue保存vlaues
              // values.push(value)
              values[index] = value

              // 如果全部成功了, 将return的promise改变成功
              if (resolvedCount === promises.length) {
                resolve(values)
              }

            },
            reason => { // 只要一个失败了, return的promise就失败
              reject(reason)
            }
          )
        })
      })
    }

    /* 
    Promise函数对象的race方法
    返回一个promise, 其结果由第一个完成的promise决定
    */
    static race = function (promises) {
      // 返回一个promise
      return new Promise((resolve, reject) => {
        // 遍历promises获取每个promise的结果
        promises.forEach((p, index) => {
          Promise.resolve(p).then(
            value => { // 一旦有成功了, 将return变为成功
              resolve(value)
            },
            reason => { // 一旦有失败了, 将return变为失败
              reject(reason)
            }
          )
        })
      })
    }

    /* 
    返回一个promise对象, 它在指定的时间后才确定结果
    */
    static resolveDelay = function (value, time) {
      // 返回一个成功/失败的promise
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          // value是promise
          if (value instanceof Promise) { // 使用value的结果作为promise的结果
            value.then(resolve, reject)
          } else { // value不是promise  => promise变为成功, 数据是value
            resolve(value)
          }
        }, time)
      })
    }

    /* 
    返回一个promise对象, 它在指定的时间后才失败
    */
    static rejectDelay = function (reason, time) {
      // 返回一个失败的promise
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          reject(reason)
        }, time)
      })
    }

  }

  // 向外暴露Promise函数
  window.Promise = Promise
})(window)
```
## 4. async与await
    1. async 函数
        函数的返回值为promise对象
        promise对象的结果由async函数执行的返回值决定
   
    2. await 表达式
        await右侧的表达式一般为promise对象, 但也可以是其它的值
        如果表达式是promise对象, await返回的是promise成功的值
        如果表达式是其它值, 直接将此值作为await的返回值
    
    3. 注意:
        await必须写在async函数中, 但async函数中可以没有await
        如果await的promise失败了, 就会抛出异常, 需要通过try...catch来捕获处理

## 5. JS异步之宏队列与微队列
![宏队列与微队列](http://vipkshttp1.wiz.cn/ks/share/resources/49c30824-dcdf-4bd0-af2a-708f490b44a1/92b8cbfb-a474-4859-943b-6048e9dc66f6/index_files/60b9ff398449db2dcfef9197e2187ae6.png)

	1. 宏列队: 用来保存待执行的宏任务(回调), 比如: 定时器回调/DOM事件回调/ajax回调
	2. 微列队: 用来保存待执行的微任务(回调), 比如: promise的回调/MutationObserver的回调
	3. JS执行时会区别这2个队列
		JS引擎首先必须先执行所有的初始化同步任务代码
		每次准备取出第一个宏任务执行前, 都要将所有的微任务一个一个取出来执行


