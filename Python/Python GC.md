
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
 改进：引人内存池机制，还有针对特定对象（PyIntObject, PyStringObject, PyDictObject, PyListObject等）的内存池机制
 - 循环引用
```
l1 = []
l2 = []
l1.append(l2)
l2.append(l1)
```
这些变量实际上并没有被任何外部变量引用，它们只是相互引用。这意味着它们不会再有人使用这组对象，应该回收它们的所占用的内存。
然而由于互相引用，这些对象的引用计数值都不为0，这意味着它们占用的内存不会被回收。
为了弥补这个缺陷，Python又引入了**标记——清除**和**分代收集**两种垃圾回收技术。

### 三色标记模型
主要用来针对可能出现循环引用的container对象。PyIntObject和PyStringObject等不可变对象主要依靠引用计数。
Python 会维护一个GC双向链表，所有的container对象都会被放到这个链表中。container对象在初始化的时候，会在其PyObject_HEAD前面，加上一个PyGC_Head，用来存储prev和next指针。
垃圾检测：
1. 扫描GC链表，找到root object
2. 从root object开始，标记reachable的对象，添加到reachable链表，其他的则归为unreachable链表

垃圾回收：
1. 移除unreachable链表中，使用了__del__函数的对象，将其放到finalizer链表中。
2. 销毁剩余unreachable链表中的对象

### 分代收集
Python中，通过数组维护了三个GC链表。分别为gc_generation[0], gc_generation[1] 和gc_generation[2]。每个链表都有相应的threshold，就是可容纳的对象数量。
当所有新分配的container对象都会被放到gc_generation[0]链表中。当对象数量超过threshold时，会触发针对当前gc_generation[0]链表的垃圾回收。
经过一轮垃圾回收，没有被回收的对象，说明他们正在被使用，会将他们移到下一代，即gc_generation[1]链表。
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI2NTM5ODA0MCwxNTc2NDc2NTIzLDc0Mz
k2NTIyMSwtNTk1NzU4NjMyLC02MzI5ODQ0MTUsLTEzOTQ1NTg5
MDcsODEyNjQ5NDEsLTExODgxNzMwMDEsODIyNTMzOTE0LC0yMD
U1NzU5NDY5LDExNzI2ODMyNDFdfQ==
-->