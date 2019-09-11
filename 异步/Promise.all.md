Promise.all使用、原理实现及错误处理？

```js
all(list) {
    return Promise((resolve, reject) => {
        let resValues = [];
        let counts = 0;
        for(let [i, p] of list) {
            resolve(p).then(res => {
                counts++;
                resValues[i] = res;
                if(counts === list.length) {
                    resolve(resValues);
                }
            }, err => {
                reject(err)
            })
        }
    })
}
```

- Promise.all()接收一个由promise任务组成的数组，可以同时处理多个promise任务，当所有的任务都执行完成时，Promise.all()返回resolve，但当有一个失败（reject），则返回失败的信息，即使其它promise执行成功，也会返回失败。

  ```js
  //请求两个url.1和url.2，当两个异步请求返回结果后，再请求第三个url.3
  const p1 = request('http://url.1')
  const p2 = request('http://url.2')
  Promise.all([p1, p2])
      .then((datas) => {
      	return request(`http://url.3?a=${datas[0]}&b=${datas[1]}`)
  	})
  	.then((data) => {
      	console.log(data)
  	})
  ```

  