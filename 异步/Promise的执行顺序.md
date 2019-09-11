Promsie是同步执行还是异步执行，then方法呢？

- Promise构造函数是同步执行的，then方法是异步执行的

```js
const promise = new Promise((resolve, reject) => {
    console.log(1)
    resolve()
    console.log(2)
})
promise.then(() => {
    console.log(3)
})
console.log(4)
```

输出顺序：1	2	4	3

