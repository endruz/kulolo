# Decorator 装饰器

- [Decorator 装饰器](#decorator-装饰器)
  - [不带参数的装饰器](#不带参数的装饰器)
  - [带参数的装饰器](#带参数的装饰器)

## 不带参数的装饰器

Python 函数是对象，有自己的属性和方法，可以作为函数的参数和返回值。
我们若想在一个函数调用前后添加一些功能，又不改变函数本身的结构，还想要把这些功能封装起来避免写重复代码。
那么根据 Python 函数的特性我们可以写出这么一个函数：

```python
>>> def decorator(fun):
...     def wrapper(*args, **kw):
...             print(f"Call {fun.__name__}()")
...             fun(*args, **kw)
...             print(f"End of {fun.__name__}() call")
...     return wrapper
...
>>> def fun():
...     print(f"This is {fun.__name__}()")
...
>>> decorator(fun)()
Call fun()
This is fun()
End of fun() call
>>> fun.__name__
'fun'
>>> fun = decorator(fun)
>>> fun
<function decorator.<locals>.wrapper at 0x000001BCABB61158>
>>> fun.__name__
'wrapper'
>>> fun()
Call fun()
This is wrapper()
End of fun() call
```

这种以函数作为参数并返回函数的函数，叫做装饰器（Decorator）。
`decorator(fun)`返回的是内置函数对象`wrapper`，当赋值给变量 fun 后，在全局变量中 fun 指向的是 `wrapper` 对象，所以两次调用输出内容不一致。
倘若原定义的 `fun()` 中使用到了 `fun.__name__`等属性的话代码会出错，所以需要 Python 内置的 `functools.wraps`装饰器来统一函数签名。具体使用如下。

```python
>>> import functools
>>> def decorator(fun):
...     @functools.wraps(fun)
...     def wrapper(*args, **kw):
...             print(f"Call {fun.__name__}()")
...             fun(*args, **kw)
...             print(f"End of {fun.__name__}() call")
...     return wrapper
...
>>> def fun():
...     print(f"This is {fun.__name__}()")
...
>>> fun
<function fun at 0x000001F7625DC1E0>
>>> decorator(fun)()
Call fun()
This is fun()
End of fun() call
>>> fun = decorator(fun)
>>> fun
<function fun at 0x000001F7628D2D08>
>>> fun.__name__
'fun'
>>> fun()
Call fun()
This is fun()
End of fun() call
```

这样我们就有一个正确的装饰器了，定义过后可以使用 @语法，更简便。

```python
>>> @decorator
... def fun1():
...     print(f"This isn't {fun1.__name__}()")
...
>>> fun1()
Call fun1()
This isn't fun1()
End of fun1() call
```

## 带参数的装饰器

装饰器也可以有参数，这样也可以丰富装饰器的功能。具体实现时只要再嵌套一层函数即可。

```python
>>> import functools
>>> def decWithArgs(text):
...     def decorator(fun):
...         @functools.wraps(fun)
...         def wrapper(*args, **kw):
...             print(f"Call {fun.__name__}()")
...             fun(*args, **kw)
...             print(f"End of {fun.__name__}() call")
...             print(text)
...          return wrapper
...     return decorator
...
>>> @decWithArgs("Finish !")
... def fun(a,b):
...     print(f"{fun.__name__}():{a}+{b}={a+b}")
...
>>> fun(1,2)
Call fun()
fun():1+2=3
End of fun() call
Finish !
```

其中 @语句相当于

```python
fun = decWithArgs("Finish !")(fun)
```
