
# 轮询

# 持久化
RabbitMQ重启之后，queue和其中的消息仍然存在。
1. 持久化queue
只需要在声明queue的时候，设置`durable=True`
2. 持久化化消息
在生产消息的时候，添加参数，

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzA5Mjc4NzQ4LC0yMDQ2NjYwMDE5LC0yMD
Q2MjM5MTQ2XX0=
-->