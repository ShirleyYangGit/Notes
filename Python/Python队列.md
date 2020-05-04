
# Python中的四种队列
参考：https://zhuanlan.zhihu.com/p/37093602

## collections.deque
线程安全的
在数据结构层面实现了队列，但是并没有应用场景方面的支持，可以看做是一个基础的数据结构

## queue.Queue
面向多生产线程、多消费线程的队列

## asyncio.Queue
面向多生产协程、多消费协程的队列

## multiprocessing.Queue
面向多成产进程、多消费进程的队列



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMzNTA0MTk4NV19
-->