# Scope

## Understanding UnboundLocalError in Python
[https://eli.thegreenplace.net/2011/05/15/understanding-unboundlocalerror-in-python](https://eli.thegreenplace.net/2011/05/15/understanding-unboundlocalerror-in-python)

> One of the stages in the compilation of Python into bytecode is building the symbol table [[4]](https://eli.thegreenplace.net/2011/05/15/understanding-unboundlocalerror-in-python#id10). An important goal of building the symbol table is for Python to be able to mark the scope of variables it encounters - which variables are local to functions, which are global, which are free (lexically bound) and so on.

符号表
将Python编译成字节码的一个阶段是构建符号表。构建符号表的一个重要目标是让Python能够标记它遇到的变量的范围——哪些变量是函数的局部变量，哪些是全局变量，哪些是自由变量(按词法绑定)，等等。

在代码运行时，解释器会根据符号表来去找变量和操作变量。

## 闭包
[https://foofish.net/python-closure.html](https://foofish.net/python-closure.html)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAyMTc2NDU0XX0=
-->