
# Python的内存管理机制

## python的内存管理架构
Python所有的内存管理都有两套，一套是debug模式下。本文只关注非debug模式下的内存管理机制。
Python内存管理机制的层次结构：
- Layer 3: 对象缓冲池机制
\[int\] \[dict\] \[list\] ... \[string\] Object-specific memory
- Layer 2: 包含GC机制
Python's object allocator (PyObj_API)
Object Memory
- Layer 1: 包装malloc, 提供统一的raw memory的管理接口
Python's raw memory allocator (PyMem_API)
Python memory (under PyMem manager's control)
- Layer 0: 操作系统内存管理接口

执行大量的malloc和free操作，会导致操作系统频繁的在用户态和核心态之间进行切换，这将严重影响python执行效率。为了提供执行效率，Python引入了内存池机制，用于管理小块内存的申请和释放，这也就是Pymalloc机制。

block
pool: 一个pool大小通常为一个系统内存页。每个pool中，block size只有一种。pool_header与其管理的内存是连续的。
arena: arena_object与其管理的内存是分离的。

## Python循环引用的垃圾收集
### 引用计数
在内存分配和释放时，加入管理引用计数的动作
优点：
 - 实时性，任何内存一旦没有指向它的引用，就会立刻被回收。
缺点：
 - 执行效率问题
 改进：引人内存池机制，还有针对特定对象（PyIntObject, ）的内存池机制

### 三色标记模型
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIyNjIwNjUwNiwtNTk1NzU4NjMyLC02Mz
I5ODQ0MTUsLTEzOTQ1NTg5MDcsODEyNjQ5NDEsLTExODgxNzMw
MDEsODIyNTMzOTE0LC0yMDU1NzU5NDY5LDExNzI2ODMyNDFdfQ
==
-->