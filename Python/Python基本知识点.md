
2. 反射
5. classmethod  staticmethod
6. 生成器，迭代器。 range和xrange
7. __new__() 在哪里定义的
 
 1. 秒杀系统，如何性能测试
 2. 性能测试

## 魔法方法
又称特殊方法，即`__init__ __len__`等前后有双下划线的方法。
这些方法其实一般不被开发者直接调用，即一般不以`a.__len__()`这样的方式使用。开发者使用`len(a)`即可，python解释器能够自动将`len(a)`解析成`a.__len__()`。

这种特殊方法能够使python代码更整洁、易于理解。


## 基本数据结构
### 字典dict和集合set
字典的key可以是哪些数据
字典的key必须是可哈希的值，可哈希的值必须是不可变的
1. 数值
2. 字符串
3. 不包含可变类型的值的tuple元组

**问题**
1. 可哈希和不可哈希
2. 什么是可变对象和不可变对象

### 列表List
在CPython中，list本质上**长度可变的数组**
`List`实际上采用的就是数据结构中的顺序表，而且是一种采用分离式技术实现的`动态顺序表`。
顺序表就是通俗意义上其他语言中所说的数组，具有一块连续的内存空间来存储数据。

Python中的`List`实现是基于数组或基于链表结构的。
从细节上看，Python中的`List`是由对象的引用组成的连续数组。指向这个数组的指针及其长度被保存在一个列表头结构中。这意味着，每次添加或删除一个元素时，由引用组成的数组需要改变大小（重新分配）。但是，并不是每次操作都需要改变数组大小的。

在添加元素时，当当前数组内存空间不够时，Python会调用list_resize() 重新设置一个新的 size（数组的内存空间大小）。它会多申请一些内存，这样也就避免了多次调用该allocate函数。

幸运的是，Python在创建这些数组时采用了指数分配，所以并不是每次操作都需要改变数组的大小。但是也因为这个原因添加或取出元素的的平摊复杂度较低。


`List`的算法效率
|function | Time complexity |
|-------:| ---------------:|
index()| O(1)
append | O(1) 
pop() | O(1) 
pop(i)| O(n) 
insert(i,item)| O(n)
del operator | O(n)
iteration | O(n)
contains(in) | O(n)
get slice[x:y] |O(k)
del slice | O(n)
set slice | O(n+k)
reverse | O(n)
concatenate | O(k)
sort | O(nlogn)
multiply | O(nk)


## 控制结构


## 面向对象
### 类
#### 属性
1. 类属性
类内和方法之外定义的属性，会放在类的__dict__中
2. 对象属性
`self.x`定义的属性，会放在实例对象的__dict__中。如果对象中添加了一个属性和类属性名称相同，那么，使用对象对该属性进行调用时，会返回对象__dict__中的值，而非类__dict__中的。

NOTE: 
在Java和C++中static关键字是指属于类但不属于类对象的变量和函数。

#### 方法
1. 静态方法
使用`@staticmethod`修饰，方法不需要传递对象或者类作为参数。一般静态方法是不对对象进行操作的。可以使用类名或者对象调用。但是，我们因为静态方法与对象无关，最好是使用类名进行调用。
在下面两种情况使用静态方法：
   - 一个方法不需要访问对象状态，其所需的参数都是通过显示参数提供
   - 一个方法只需访问类的静态域。（类的静态域是所有实例共享的，不属于任何独立对象。Python中类的静态域可以理解为类的__dict__）

2. 类方法
使用`@classmethod`修饰，方法第一个参数是`cls`。

3. 对象方法
方法第一个参数是	`self`。一般对实例对象进行操作的方法。

### 私有化
- `x`: 公有属性和方法名 
- `_x`: 这种方式的命名视为“受保护的”属性，有时也可以约定为私有属性。Python解释器不会对其进行**名称改写**。很多程序员严格遵守约定，不会在类外部访问这种属性。不过在模块中，顶层名称使用一个前导下划线的话，的确会受影响：对`from mymod import *` 来说前缀为下划线的名称不会被导入。然而，依旧可以使用`from mymod import _privatefunc`将其导入。
- `__x`: 私有属性或方法名。Python解释器会对这种名称进行**名称改写**，使外部无法通过该名对其进行访问。（但是如果直接访问改写后的名称`_classname__x`，仍然是可以的。）
- `__x__`: 一般是Python特殊方法名，不建议自定义这种方法

基于类的访问权限
一个方法可以访问所属类的所有对象的私有数据。比如下面的isAgeEquals方法。
```
class Employee:
    def __init__(self, name, age):
        self.name = name
        self.__age = age
        
    def isAgeEquals(self, Employee: b):
        return self.__age == b.__age
```
对于类Employee的对象b，b.__dict__为：
```{'name': 'Bob', '_Employee__age': 30}```

## 反射
[https://www.jianshu.com/p/628f61f01a54](https://www.jianshu.com/p/628f61f01a54)
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU0MzA3MjExMywyNDg2MTcwLDIyNTQ1OT
MzNywtMTk1MTg3MTkxNSw5MTYwNzQ4MDEsNjMwNTAwNjQwLC0x
MTE4OTA4NjU0LDE0MzkzMTg0ODcsLTYzMzEyMTM3MiwtMTY4Nz
AyOTEzNiwtMjAyMzUxNjQxNl19
-->