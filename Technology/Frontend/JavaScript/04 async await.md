本质：只是 Promise 的更好写法，它没有创造新东西，它只是让异步代码看起来更像同步代码。

### `async`

`async` 放在函数前面，指这个函数是异步函数：

```js
async function main() { 
 
}
```

> 🌟 async 函数一定会返回一个 Promise。

比如：

```js
async function test() {  
  return 123  
}
```

并非返回 `123`，返回的是：`Promise.resolve(123)`，也就是一个“成功的 Promise”

```js
async function test() {  
  return 123  
}  
  
test().then((value) => {  
  console.log(value)  
})
```

会打印：`123`

### `await` 

`await` 的意思可以先粗暴理解成：等一下，等这个 Promise 出结果。

```js
const response = await fetch("某个网址")
```

意思是：先发请求，等请求结果回来，再把结果放进 `response`，然后代码继续往下走，避免一堆 `.then()` 套起来。

### 例子

```js
async function main() {
  const userResponse = await fetch("https://jsonplaceholder.typicode.com/users/1")
  const user = await userResponse.json()

  const postsResponse = await fetch(
    `https://jsonplaceholder.typicode.com/posts?userId=${user.id}`
  )
  const posts = await postsResponse.json()

  console.log("用户名:", user.name)
  console.log("文章数量:", posts.length)
  console.log("第一篇文章标题:", posts[0].title)
}

main()
```

```js
console.log('script start') 

async function async1() { 
	await async2() 
	console.log('async1 end') 
} 

async function async2() { 
	console.log('async2 end') 
} 

async1() 

setTimeout(() => { 
	console.log('setTimeout') 
}, 0) 

new Promise((resolve) => { 
	console.log('promise') 
	resolve() 
}).then(() => { 
	console.log('then1') 
}).then(() => { 
	console.log('then2') 
}) 

console.log('script end')
```

输出：
```js
script start 
async2 end 
promise 
script end 
async1 end 
then1 
then2 
setTimeout
```
### `try...catch`

Promise 写法里用 `.catch()`，async / await 里通常用 `try...catch`

```js
async function main() {  
  try {  
    const response = await fetch("https://jsonplaceholder.typicode.com/users/1")  
    const user = await response.json()  
    console.log(user.name)  
  } catch (error) {  
    console.log("出错了")  
    console.log(error)  
  }  
}  
  
main()
```

`try` 里放你觉得可能出错的代码，如果出错，跳到 `catch`

### async / await 和 then的关系

```js
const user = await getUser()
```

本质上很接近：

```js
getUser().then((user) => {  
})
```

await 背后还是 Promise

### 注意

> 🌟 `await`只能写在`async`函数里面

模版：

```js
async function main() {
  try {
    const response = await fetch("一个网址")
    const data = await response.json()
    console.log(data)
  } catch (error) {
    console.log("请求失败:", error.message)
  }
}

main()
```