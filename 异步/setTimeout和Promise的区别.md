setTimeout、Promise、Async/Await的区别？

事件循环中分为宏任务队列和微任务队列：

-  setTimeout的回调函数放到宏任务队列里，等到执行栈清空以后执行
- promise.then里的回调函数会放到相应宏任务的微任务队列里，等宏任务里面的同步代码执行完再执行
- async函数表示函数里面**可能**会有异步方法，await后面跟一个表达式，async方法执行时，遇到await会立即执行表达式，然后把表达式后面的代码放到微任务队列里，让出执行栈让同步代码先执行

## setTimeout

```js
console.log(1);
setTimeout(function() {
	console.log(2)
})
console.log(3)
```

输出顺序：1	3	2

## Promise

Promise本身是**同步的立即执行函数**，当执行resolve或者reject的时候，此时是异步操作，会先执行then/catch等，当主栈完成后，才会去调用resolve/reject中存放的方法

```js
console.log(1)
let promise1 = new Promise(function(resolve) {
    console.log(2)
    resolve()
    console.log(3)
}).then(function() {
    console.log(4)
});
setTimeout(function() {
    console.log(5);
})
console.log(6)
```

输出顺序：1	2 	3	6	4	5

当js主线程直行道Promise对象时：

- promise1.then()的回调就是一个task
- promise1是resolved或rejected：那这个task就会放入当前事件循环回合的microtask queue
- promise1是pending：这个task就会放入事件循环的未来的某个（可能下一个）回合的microtask queue
- setTimeout的回调也是个task，它会被放入macrotask queue，即使是0ms的情况



## async/await

```js
async function async1() {
    console.log(1)
    await async2();
    console.log(2)
}
async function async2() {
    console.log(3)
}
console.log(4)
async1();
console.log(5)
```

输出顺序：4	1	3	5	2

async函数返回一个Promise对象，当函数执行的时候，一旦遇到await就会先返回，等到触发的异步操作完成，再执行函数体内后面的语句。可以理解为，是让出了线程，跳出了async函数体。

await的含义为等待，也就是async函数需要等待await后的函数执行完成，并且有了返回结果（Promise对象）之后，才能继续执行下面的代码。await通过返回一个Promise对象来实现同步的效果。