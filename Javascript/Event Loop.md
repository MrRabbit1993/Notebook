## 事件循环Event loop

js 单线程非阻塞脚本语言

参考地址：https://zhuanlan.zhihu.com/p/33058983
### 执行栈与事件队列
同步类型代码丢入执行栈，异步丢入事件队列

执行栈执行完毕，取事件队列的的来执行，然后又丢入执行栈来执行。重复执行就形成事件循环
### 微任务（micro task）和宏任务（macro task）
### 执行顺序：（微任务比宏任务先执行）
当前执行栈执行完毕时会立刻先处理所有微任务队列中的事件，然后再去宏任务队列中取出一个事件。同一次事件循环中，微任务永远在宏任务之前执行
#### 宏任务：
setInterval、setTimeout
#### 微任务 (new Promise是同步)
new Promise的then、new MutationObserver
```js
setTimeout(function () {
  console.log(1)
})

new Promise(function (resolve, reject) {
  console.log(2)
  resolve(3)
}).then(function (val) {
  console.log(val)
})
console.log('console')
// 得到 2，console,3,1
```
```js
setTimeout(() => {
  console.log('定时器')
  setTimeout(() => {
    console.log('setTimeout定时器')
  }, 0)
}, 0)
Promise.resolve().then((value) => {
  console.log('Promise')
  setTimeout(() => {
    console.log('Promise定时器')
  }, 0)
})
console.log('console')
// 得到 console,Promise,定时器，Promise定时器，setTimeout定时器
//原理：console是同步，Promise.then和setTime属于同一事件循环，Promise.then微任务，setTime宏任务,同一事件循环完毕。进入下一次的事件循环（内嵌的setTimeout）
```