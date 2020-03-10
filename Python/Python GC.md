
# Python的内存管理机制

## python的内存管理架构
Python所有的内存管理都有两套，一套是debug模式下。本文只关注非debug模式下的内存管理机制。
Python内存管理机制的层次结构：
一共有四层：
- Layer 0: 操作系统内存管理接口
- Layer 1: 包装Layer 0, 提供统一的raw memory的管理接口
Python's raw memory allocator (PyMem_API)
Python memory (under PyMem manager's control)
- Layer 2: Python's object allocator (PyObj_API)
Object Me
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2NTg3NTgwNDIsLTIwNTU3NTk0NjksMT
E3MjY4MzI0MV19
-->