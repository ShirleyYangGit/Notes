
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


### 问题
1. 可哈希和不可哈希
2. 什么是可变对象和不可变对象

## 控制结构


## 面向对象
### 私有化

### 类方法
静态方法
类方法
方法
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQzOTMxODQ4NywtNjMzMTIxMzcyLC0xNj
g3MDI5MTM2LC0yMDIzNTE2NDE2XX0=
-->