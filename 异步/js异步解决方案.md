js异步解决方案的发展历程及优缺点？

1. 回调函数（callback）

   ```js
   setTimeout(() => {
   	// callback函数体
   }, 1000)
   ```

   缺点：回调地狱，不能用try/catch捕获错误，不能return

   回调地狱的根本问题在于：

   - 缺乏顺序性：回调地狱导致的调试困难，和大脑的思维方式不符

   - 嵌套函数存在耦合性，一旦有所改动，就会牵一发而动全身，即控制反转

   - 嵌套函数过多的话，很难处理错误

     ```js
     ajax('XXX1', () => {
         // callback 函数体
         ajax('XXX2', () => {
             // callback 函数体
             ajax('XXX3', () => {
                 // callback 函数体
             })
         })
     })
     ```

     

   优点：解决了同步的问题（只要有一个任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的执行）

2. Promise

   Promise就是为了解决callback的问题而产生的。

   Promise实现了链式调用，也就是说每次then后返回的都是一个全新Promise，如果在then中return，return的结果会被Promise.resolve()包装

   优点：解决了回调地域的问题

   ```js
   ajax('XXX1')
     .then(res => {
         // 操作逻辑
         return ajax('XXX2')
     }).then(res => {
         // 操作逻辑
         return ajax('XXX3')
     }).then(res => {
         // 操作逻辑
     })
   ```

   缺点：无法取消Promise，错误需要通过回调函数来捕获

3. Generator

   特点：可以控制函数的执行，可以配合co函数库使用

   ```js
   function *fetch() {
       yield ajax('xxx1', () => {})
       yield ajax('xxx2', () => {})
       yield ajax('xxx3', () => {})
   }
   let it = fetch()
   let result1 = it.next()
   let result2 = it.next()
   let result3 = it.next()
   ```

4. Async/await

   async/await是异步的终极解决方案

   优点：代码清晰，不用像Promise写一大堆then链，处理了回调地狱的问题

   缺点：await将异步代码改造成同步代码，如果多个异步操作没有依赖性而使用await会导致性能上的降低

   ```js
   async function test() {
       // 以下代码没有依赖性的话，完全可以使用 Promise.all 的方式
       // 如果有依赖性的话，其实就是解决回调地狱的例子了
       await fetch('XXX1')
       await fetch('XXX2')
       await fetch('XXX3')
   }
   ```

   看一个例子：

   ```js
   let a = 0;
   let b = async() => {
   	a = a + await 10;
   	console.log('2', a) // '2', 10
   }
   b();
   a++;
   console.log('1', a) // '1' 1
   ```

   分析如下：

   - 首先函数b执行，在执行到await 10之前变量a还是0，因为await内部是吸纳了generator，generator会保留堆栈中东西，所以这时候a = 0 被保存了下来

   - 因为await是异步操作，后面的表达式不返回Promise的话，就会包装成Promise.resove(返回值)，然后执行函数外的同步代码

   - 同步代码执行完毕后开始执行异步代码，将保存下来的值拿出来使用，这时候 a = 0 + 10

     

5. 