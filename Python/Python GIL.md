
# Python GIL

Python中的线程受GIL互斥锁限制，只有获取了GIL的线程才能够使用Python解释器执行代码。

1. 线程何时释放GIL
Python内部通过软件模拟了操作系统的时钟中断机制，也有一套类似的执行py_ticker(100)条指令后，自动释放GIL，通过操作系统唤醒下一个等待线程
3. 如何从等待的线程池中选择下一个执行的线程
操作系统决定

如果没有调用到多线程，GIL不会被初始化，程序将一直执行，不会被GIL打断。

在线程初始化成功前，操作系统线程调度并没有和Python的线程调度一致。
当所有的线程都初始化成功后，Python的线程调度才和操作系统的线程调度一致。确切地说，是操作系统的线程调度会受GIL限制，根据GIL的获取和释放来进行线程调度。

## Python的线程调度
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTMwMDI5NjM5LC0xMzcwNTY3MDcxLC0xNz
Y2MTQ5NzA5LC03MzMzNTU0MTldfQ==
-->