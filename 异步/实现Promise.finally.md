模拟实现一个Promise.finally

```js
Promise.prototype.finally = function(callback) {
    let P = this.constructor;
    return this.then(
    	value => P.resolve(callback()).then(() => value),
        reson => P.resolve(callback().then(() => { throw reson }))
    );
}
```

