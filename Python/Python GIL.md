
# Python GIL

Python中的线程受GIL互斥锁限制，只有获取了GIL的线程才能够使用Python解释器执行代码。

1. 线程何时释放GIL
Python内部通过软件模拟了操作系统的时钟中断机制，也有一套类似的执行_Py_Ticker(100)条指令后，自动释放GIL，通过操作系统唤醒下一个等待线程
3. 如何从等待的线程池中选择下一个执行的线程
操作系统决定

如果没有调用到多线程，GIL不会被初始化，程序将一直执行，不会被GIL打断。

在线程初始化成功前，操作系统线程调度并没有和Python的线程调度一致。
当所有的线程都初始化成功后，Python的线程调度才和操作系统的线程调度一致。确切地说，是操作系统的线程调度会受GIL限制，根据GIL的获取和释放来进行线程调度。

## Python的线程调度
标准调度

阻塞调度
在线程通过阻塞调度切换时，Python内部的_Py_Ticker依然会保持，不会被重置为100。只有标准调度才会重置这个Python的模拟时钟。

## 子线程的销毁
主线程销毁比子线程要多销毁python运行时的环境。
子线程是要销毁线程状态对象，释放GIL

##  用户级的互斥锁
lock = thread.allocate_lock()

## 问题
面试题：描述Python GIL的概念，以及它对python多线程的影响？并阐明多线程抓取程序是否可以比单线程性能有提升，并解释原因。
1. Python语言和GIL没有关系，GIL是CPython解释器中的问题，JPython中就没有。由于历史原因，CPython解释器难以移除GIL。
2. GIL：全局解释锁。每个线程
3. Python中的线程受GIL互斥锁限制，只有获取了GIL的线程才能够使用Python解释器执行代码。

Python多线程的程序对于I/O密集型程序，还是比单线程快
计算密集型：进程
IO密集型：线程、协程

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3MjA3ODkxMTIsMTI4NDkxNjQzMywtMT
M0NzU5OTAxMywtOTMzNTAyMjkxLDE2NjQzNjYxNzQsOTMwMDI5
NjM5LC0xMzcwNTY3MDcxLC0xNzY2MTQ5NzA5LC03MzMzNTU0MT
ldfQ==
-->