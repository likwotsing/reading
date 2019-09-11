async/await如何通过同步的方式实现异步？

- async/await是参照Generator封装的一套异步处理方案，可以理解为Generator的语法糖
- Generator又依赖于Iterator
- Iterator的思想又来源于单向链表