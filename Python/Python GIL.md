
# Python GIL

1. 线程何时释放GIL
Python模拟了操作系统的时钟机制，也有一套类似的执行NUM步骤后，自动释放GIL，通过操作系统唤醒下一个等待线程
3. 如何从等待的线程池中选择下一个执行的线程
操作系统决定

如果没有调用到多线程，GIL不会被初始化，程序将一直执行，不会被GIL打断。

Python的线程调度
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI3NDcxMDEzMCwtMTc2NjE0OTcwOSwtNz
MzMzU1NDE5XX0=
-->