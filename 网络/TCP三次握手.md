TCP三次握手和四次挥手？

- 三次握手：保证client和server均让对方知道自己的连接和发送能力没问题二保证的最小次数
  - 第一次：client发送给server，只能server判断出client具备发送能力
  - 第二次：server发送给client，client可以判断出server具备发送能力和接收能力
  - 第三次：client发送给server，client让server知道自己接收能力没问题，此时，client和server双方均确认了自己的接收和发送能力没有问题
  - 为了保证后续的握手是为了应答上一个握手，每次握手都会带一个标识seq，后续的ACK都会对这个seq进行加一来进行确认
- 四次挥手：多了一层等待服务器再一次响应回复相关数据的过程
- MSL：报文最大生存时间（Maximum Segment Lifetime），RFC 793中规定MSL为2分钟，实际中常用的是30秒，1分钟和2分钟
- 2MSL：TCP的TIME_WAIT状态也称为2MSL等待状态。当TCP的一段发起主动关闭，在发出最后一个ACK包对方没收到，那么对方在超时后将重发第三次握手的FIN包，主动关闭端接到重发的FIN包后可以再发一个ACK应答包。
- 在TIME_WAIT状态时两端的端口不能使用，要等到2MSL时间结束才可继续使用。当连接处于2MSL等待阶段时任何迟到的报文段都将被丢弃。不过在实际应用中可以通过设置SO_REUSEADDR选项达到不必等待2MSL时间结束再使用此端口。

![三次握手四次挥手](https://likwotsing.github.io/images/三次握手四次挥手.png)