webpack热更新原理，是如何做到在不刷新浏览器的前提下更新页面的？

1. 当修改了一个或多个文件

2. 文件系统接收更改并通知webpack

3. webpack重新编译构建一个或多个模块，并通知HMR服务器进行更新

4. HMR Server使用webSocket通知HMR runtime需要更新，HMR运行时通过HTTP请求更新jsonp

5. HMR运行时替换更新中的模块

   如果确定这些模块无法更新，则触发整个页面刷新。



- hot-module-replacement-plugin包给webpack-dev-server提供了热更新的能力，它们两者是结合使用的，单独写两个包也是出于功能的解耦来考虑的
  - webpck-dev-server（WDS）的功能提供bundle server的能力，就是生成的bundle.js文件可以通过localhost:xxx的方式去访问，另外WDS也提供liverload（浏览器的自动刷新）
  - hot-module-replacement-plugin的作用是提供HMR的runtime，并且将runtime注入到bundle.js代码里面去。一旦磁盘里面的文件修改，那么HMR server会将有修改的js module信息发送给HMR runtime，然后HMR runtime去局部更新页面的代码。因此这种方式可以不用刷新浏览器。



- 参考这个[HMR](https://juejin.im/post/5c86ec276fb9a04a10301f5b)

1. 在页面使用socket和http-server建立链接
2. module.hot.accpet来添加HMR的js模块
3. 当对应模块发生变化，发送socket请求和client的chunk头进行比较
4. 如果有变化，头部追加script脚本chunk