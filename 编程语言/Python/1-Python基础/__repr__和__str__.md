# `__repr__`和`__str__`

学过 Java 的同学都知道 `toString` 这个方法，在学习 Python 时大多数会被告知 `__str__` 这个函数和 Java 的 `toString` 方法一样。直到遇见 `pprint` ，我发现原来事情并不简单。

贴出一段代码

```python
from pprint import pprint
class STR:
    data = "test __str__"
    def __str__(self):
        return self.data

ts = STR()
print(ts)
pprint(ts)
```

输出结果：

```log
test __str__
<__main__.STR object at 0x00000215A388C0B8>
```

在我查看 pretty print 源码之后发现这样一段代码

```python
p = self._dispatch.get(type(object).__repr__, None)
```

这个 `__repr__` 又是什么？？为什么不用 `__str__` ？？？
头顶着一万个问号，我去查了查 Python 文档。
>`object.__repr__(self)`
>>Called by the repr() built-in function to compute the “official” string representation of an object. If at all possible, this should look like a valid Python expression that could be used to recreate an object with the same value (given an appropriate environment). If this is not possible, a string of the form <...some useful description...> should be returned. The return value must be a string object. If a class defines `__repr__()` but not `__str__()`, then `__repr__()` is also used when an “informal” string representation of instances of that class is required.
>>
>>This is typically used for debugging, so it is important that the representation is information-rich and unambiguous.
>
>`object.__str__(self)`
>>Called by str(object) and the built-in functions format() and print() to compute the “informal” or nicely printable string representation of an object. The return value must be a string object.
>>
>>This method differs from `object.__repr__()` in that there is no expectation that `__str__()` return a valid Python expression: a more convenient or concise representation can be used.
>>
>>The default implementation defined by the built-in type object calls `object.__repr__()`.

大概意思有以下几点：

1. `__repr__`正式，`__str__` 非正式。
2. `__str__`主要由 `str()`,`format()`和`print()`三个方法调用。
3. 若定义了`__repr__`没有定义`__str__`，那么本该由`__str__`展示的字符串会由`__repr__`代替。
4. `__repr__`主要用于调试和开发，而`__str__`用于为最终用户创建输出。
5. `__repr__`看起来更像一个有效的 Python 表达式，可用于重新创建具有相同值的对象（给定适当的环境）。

什么是正式？什么是非正式？光听文档上还是太抽象了，那我们就来看看几个 Python 内置对象的`__repr__`、`__str__`区别。
*（PS：因为 Python 内置函数 `repr()` 和 `str()` 分别调用对象的`__repr__`和`__str__`，所以这边就用这两个函数来举例。）*

```python
import datetime
s = 'hello'
d = datetime.datetime.now()
print(str(s))
print(repr(s))
print(str(d))
print(repr(d))
```

输出结果：

```log
hello
'hello'
2018-11-29 22:39:18.014587
datetime.datetime(2018, 11, 29, 22, 39, 18, 14587)
```

可以看出`repr()`更能显示出对象的类型、值等信息，对象描述清晰的。
而`str()`能够让我们最快速了解到对象的内容，可读性较高。

还有哦，在 Python 交互式命令行下直接输出对象默认使用的是`__repr__`。
