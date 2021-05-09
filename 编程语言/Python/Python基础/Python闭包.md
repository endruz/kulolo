# 闭包 Closure

- [闭包 Closure](#闭包-closure)
  - [含义](#含义)
  - [引用环境可以是全局变量](#引用环境可以是全局变量)
  - [闭包函数的外层也可以是模块](#闭包函数的外层也可以是模块)

## 含义

在维基百科下闭包的释义如下：

>在计算机科学中，闭包（英语：Closure），又称词法闭包（Lexical Closure）或函数闭包（function closures），是引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外。所以，有另一种说法认为闭包是由函数和与其相关的引用环境组合而成的实体。闭包在运行时可以有多个实例，不同的引用环境和相同的函数组合可以产生不同的实例。

```python
>>> def outer():
...     def inner():
...         print(s)
...     s = "Closure"
...     return inner
...
>>> innerfun = outer()
>>> innerfun()
Closure
```

上面的例子中，变量`innerfun`接收的是`outer()`返回的`inner`函数对象，脱离了外部函数`outer()`，还是可以使用外部函数`outer()`的变量`s`。
`inner`和`s`组成了一个闭包。
从这里也可以看出 Python 装饰器是闭包的一种应用场景。

## 引用环境可以是全局变量

```python
>>> s = "Closure"
>>> def outer():
...     def inner():
...             print(s)
...     return inner
...
>>> innerfun = outer()
>>> innerfun()
Closure
```

## 闭包函数的外层也可以是模块

`test.py`

```python
g = "global in test"
def fun():
    print(g)
```

`test1.py`

```python
from test import fun

# print(g)
fun()

from test import g
print("g in test is",g)
```

这个例子中`fun`和`g`组成一个闭包。
