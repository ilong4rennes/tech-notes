### 引入

**问题**：如果需要下载数据，JS不可能停在那里等，所以JS会开始下载，继续执行别的代码，等下载完成再通知你——异步

**最早的解决方案：callback function回调函数**（事情完成以后再调用这个函数）
- 问题：callback hell 一层套一层 代码很难读

**解决方案：Promise**
- 代表未来结果的对象
- 比如你在等外卖，外卖还没到，但系统会给到一个状态：正在配送/已完成/配送失败，Promise 也是一样

### Promise的三个状态

1. pending 等待
2. fulfilled 成功
3. rejected 失败

状态变化只有一次：`pending → fulfilled` 或`pending → rejected`，不能再变

### API

#### 1. `new Promise(executor)`

```javascript
const p = new Promise((resolve, reject) => {
  // 做一些异步操作
})
```

里面这个函数叫 **executor**，执行器。**executor 会立刻执行** 比如：

```js
const p = new Promise((resolve, reject) => {  
  console.log("开始了")  
  resolve("ok")  
})
```

你一创建 Promise，`开始了` 就会立刻打印。

#### 2. `promise.then(onFulfilled, onRejected)`

`then` 用来处理成功结果，也能顺手处理失败。

```js
p.then(  
  (value) => {  
    console.log("成功", value)  
  },  
  (reason) => {  
    console.log("失败", reason)  
  }  
)
```

但实际开发中，第二个参数很少用，大家更常用 `.catch()` 单独接失败。

更常见写法：

```js
p.then((value) => {  
  console.log(value)  
})
```

#### 3. `promise.catch(onRejected)`

专门处理失败。

```js
p.catch((err) => {  
  console.log(err)  
})
```

它本质上约等于：`p.then(null, onRejected)`，但平时直接记 `catch` 就行。

#### 4. `promise.finally(onFinally)`

最后总会执行。

```js
p.finally(() => {  
  console.log("收尾")  
})
```

`finally` 一般**不拿结果**，它不是用来改成功值或失败原因的，主要用来做清理工作。

#### 5. `Promise.all([...])`

 等一组 Promise 全都成功，才算成功。只要有一个失败，就直接失败。 

```js
Promise.all([p1, p2, p3])  
  .then((results) => {  
    console.log(results)  
  })  
  .catch((err) => {  
    console.log(err)  
  })
```

成功时，返回一个数组，顺序和你传进去的一样。

```js
Promise.all([  
  Promise.resolve("A"),  
  Promise.resolve("B"),  
  Promise.resolve("C")  
]).then((results) => {  
  console.log(results)  
})
```

输出：`["A", "B", "C"]`

失败时，只要其中一个 reject，整个 `all` 就 reject。

适合你要同时请求多个接口，并且**一个都不能少**。

### 例子

```javascript
let p = new Promise(function(resolve, reject) {
	setTimeout(function() {
		resolve("data ready")
	}, 1000)
})

p.then(function(result) {
	console.log(result)
})
```

流程：
1. 创建 Promise  
2. 启动定时器  
3. 一秒后调用 resolve  
4. Promise 状态变成 fulfilled  
5. then 被执行，输出：data ready

### error例子

```javascript
let p = new Promise(function(resolve, reject) {
	reject("error")
})

p.catch(function(err) {
	console.log(err)
})
```