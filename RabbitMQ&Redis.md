# RabbitMQ
## 轮询
当多个worker消费一个队列里的消息时，会采用轮询机制，就是每个worker轮流从队列里拿取消息进行消费。
默认情况下RabbitMQ根据worker执行完消息后返回的ack来判断消息是否被成功执行。如果没有收到ack，RabbitMQ会把这个消息发给另一个worker。
注意：当消息执行完成，worker也不是默认发送ack，需要添加返回ack的代码。

## 持久化
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
## 广播模式
通过exchange的类型来控制消息的发送机制
* exchange type = fanout
所有bind到此exchange的queue都可以接收到消息。
`routing_key`可以代表queue name，也可以代表和queue相关的key信息。在fanout广播模式下，producer可以将该值设为空。consumer需要为queue设置“排他的”属性。
该模式下，所有绑定了该exchange的、并且在工作的worker都可以收到该消息。没有绑定该exchange的、或者没有工作的worker是收不到消息的。
* exchange type = direct
通过`routing_key`和exchange决定的那个唯一的queue可以接收消息
在该模式下，worker在queue_bind的时候也可以为queue设置routing_key，exchange会根据routing_key来将消息分发到worker对应的队列。
* exchange type = topic
所有符合`routing_key`（此时可以是一个表达式）的`routing_key`所bind的queue可以接收消息

## RPC
Remote Procedure Call
远程调用执行，producer发一条指令到broker rpc_queue队列，consumer从broker rpc_queue队列拿到指令去执行，然后将结果返回到broker，然后producer再从broker中拿到结果
问题：producer如何知道从哪里拿取某一条指令的结果呢？
答：producer在发送的时候，附带reply_to和correction_id两个参数
reply_to告诉consumer返回的结果放到哪个queue
correction_id值用来标识哪一条指令的结果

# Redis
缓存系统，
mongodb: 默认持久化，同时存在内存和硬盘中
Redis: 半持久化，默认数据只存到内存中，需要手动持久化，存到硬盘中
memcache: 只存在内存，不能持久化。

## String操作
```
set key value
get key value
APPEND key value
getset key value
mset key value [key value ...]
# 用一个二进制位来表示用户是否在线，二进制位置即用户id
setbit key offset 0/1
getbit key offset # 和判断某个用户是否在线
bitcount key # 统计这个value中二进制表示中有多少1，可以应用于统计在线用户数
```
## Hash操作
```
hset n1 key value
HGETALL n1 # 获取n1桶里的所有key和value的值
HGET n1 key # 获取n1桶里key对应的值
HKEYS n1 # 获取n1桶里所有的key
HVALS n1 #获取n1桶里所有的value

hmset n2 key value [key value ...]
hmget n2 field [field ...]

hlen n1 # key数量
hexists n1 k1 # 判断是否存在
```
## List
```
lpush key value1 value2 ... # 从左边加入
lrange key 0 -1 # 获取所有value
rpush key value1 value2 ... # 从右边加入
LINSERT key BEFORE|AFTER value new_value
lset key index new_value
lpop key 
blpop key value
```
## Set
```
# 无序集合
sadd key value1 ...
smembers key
sdiff key1 key2
# 有序集合
zadd key score value ...
```

## 订阅发布
publish 
subscribe


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNTQxNjIxODksLTU1NDQ3NjY0OSwtMT
QxNDM5MTE3MywtMTE3Mzk1NDgwMywxNTkzOTQxMDcwLC0xMzcx
ODg2MjEsLTE1NjAxODU4OTYsLTE0NTM2NTA5OTEsLTIxMDIzMD
Q1OTQsMTY0MDUxMjEyOV19
-->