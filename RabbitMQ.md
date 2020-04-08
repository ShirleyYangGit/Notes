
# 轮询
当多个worker消费一个队列里的消息时，会采用轮询机制，就是每个worker轮流从队列里拿取消息进行消费。
默认情况下RabbitMQ根据worker执行完消息后返回的ack来判断消息是否被成功执行。如果没有收到ack，RabbitMQ会把这个消息发给另一个worker。
注意：当消息执行完成，worker也不是默认发送ack，需要添加返回ack的代码。

# 持久化
RabbitMQ重启之后，queue和其中的消息仍然存在。
1. 持久化queue
只需要在声明queue的时候，设置`durable=True`
2. 持久化化消息
在生产消息的时候，添加参数，	
```
# make message persistent
properties = pika.BasicProperties(
    delivery_mode = 2
    )
```
# 广播模式
通过exchange的类型来控制消息的发送机制
* exchange type = fanout
该模式下，所有绑定了该exchange的、并且在工作的worker都可以收到该消息。没有绑定该exchange的、或者没有工作的worker是收不到消息的。
* exchange type = direct
consume
* 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYzNDQzMjg4NSwxMDY2NDE0MTMsLTIwND
Y2NjAwMTksLTIwNDYyMzkxNDZdfQ==
-->