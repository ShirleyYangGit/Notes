
# 轮询
当多个worker消费一个队列里的消息时，会采用轮询机制，就是每个worker轮流从队列里拿取消息进行消费。
默认情况下RabbitMQ根据worker执行完消息后返回的ack来判断消息是否被成功执行。如果没有收到ack，RabbitMQ会把这个消息发给另一个worker。

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

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NzE2NDE0NjIsLTIwNDY2NjAwMTksLT
IwNDYyMzkxNDZdfQ==
-->