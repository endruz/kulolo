# Python 迭代器和生成器

- [Python 迭代器和生成器](#python-迭代器和生成器)
  - [Iterable 迭代器](#iterable-迭代器)
    - [Iterable](#iterable)
    - [Iterator](#iterator)
    - [Iterable 和 Iterator 关系](#iterable-和-iterator-关系)
    - [判断 Iterable 和 Iterator](#判断-iterable-和-iterator)
    - [for 循环](#for-循环)
  - [Generator 生成器](#generator-生成器)
    - [生成器表达式](#生成器表达式)
    - [yield 关键字](#yield-关键字)
    - [yield from 用法](#yield-from-用法)

## Iterable 迭代器

在学习 Python 迭代器之前要先了解 Iterable 的含义。

### Iterable

Iterable 为可迭代对象，是一个有明确元素个数并且可迭代的对象，如 list, string, tuple, set, dic...

可迭代对象实现了 `__iter__()`，且返回一个对应的迭代器（如 list 对应的迭代器就是 list_iterator，dic 对应 dict_keyiterator）。

### Iterator

Iterator 为迭代器，是一个不知道元素个数并且可迭代的对象。使用`next()`获取下一个元素（**无法回退**），直到没有值可以访问，抛出 StopIteration 异常。

迭代器实现 `__iter__()`和`__next__()`。`__iter__()`返回 Iterator 对象本身（即 self），使用`next()`时会调用`__next__()`返回下一个值。

### Iterable 和 Iterator 关系

Iterator 继承 Iterable ，Iterable 可使用 `iter()` 获取自身的迭代器（其实是调用自身的`__iter__()`）。

### 判断 Iterable 和 Iterator

因为 `isinstance()` 会认为子类是一种父类类型，所以我们可以使用`isinstance()`来判断一个对象是 Iterable 还是 Iterator。

```python
>>> from collections import Iterator,Iterable
>>> lis = [1,2]
>>> isinstance(lis, Iterable)
True
>>> isinstance(lis, Iterator)
False
>>> dic = {"a":1,"b":2,"c":3,"d":4}
>>> isinstance(dic, Iterable)
True
>>> isinstance(dic, Iterator)
False
>>> iter_list = iter(lis)
>>> len(lis)
2
>>> len(iter_list)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: object of type 'list_iterator' has no len()
>>> isinstance(iter_list, Iterable)
True
>>> isinstance(iter_list, Iterator)
True
>>> next(iter_list)
1
>>> next(iter_list)
2
>>> next(iter_list)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>> iter_dic = iter(dic)
>>> isinstance(iter_dic, Iterable)
True
>>> isinstance(iter_dic, Iterator)
True
>>> for x in iter_dic:
...     x
...
'a'
'b'
'c'
'd'
>>> for x in iter_dic:
...     x
...
>>>
```

### for 循环

Python 的 for 循环本质上就是通过获取 Iterator 不断调用 `next()` 实现的。

```python
lis = [1, 2, 3, 4, 5]
for x in lis:
    pass
```

等价于：

```python
it = iter(lis)
while True:
    try:
        x = next(it)
    except StopIteration:
        break
```

Iterator 只能遍历一次，其 `__iter__()` 返回自身，所以只能使用一次 for 循环。Iterable 的 `__iter__()` 每次都会得到一个新的 Iterator，故 Iterable 可以 for 循环多次。

## Generator 生成器

生成器本质上是一种一边循环一边计算的迭代器。

### 生成器表达式

之前说数据类型的时候说到了列表生成式、集合生成式、字典生成式。但是却没说到元组生成式，原因是“元组生成式”得到的并不是元组，而是生成器，所以应该称为生成器表达式。

```python
>>> from collections import Iterator
>>> [n**2 for n in [1,2,3]]
[1, 4, 9]
>>> (n**2 for n in [1,2,3])
<generator object <genexpr> at 0x0000022CBC958D68>
>>> g=(n**2 for n in [1,2,3])
>>> next(g)
1
>>> next(g)
4
>>> next(g)
9
>>> next(g)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>> isinstance(g,Iterator)
True
>>> type(g)
<class 'generator'>
```

### yield 关键字

普通函数用 return 返回一个值，然而在 Python 中还有一种函数，用关键字 yield 来返回值，这种函数叫生成器函数，函数被调用时会返回一个生成器对象。

```python
>>> def fun(n):
...     while n:
...         yield n
...         n-=1
...
>>> g = fun(5)
>>> type(g)
<class 'generator'>
>>> for i in range(6):
...     next(g)
...
5
4
3
2
1
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
StopIteration
```

在使用生成器函数时，每次遇到 yield 时函数会暂停并保存当前所有的运行信息，返回 yield 的值, 并在下一次执行 next() 方法时从当前位置继续运行。

生成器的好处就是无需一次性把所有元素加载到内存，只有迭代获取元素时才返回该元素。

### yield from 用法

`yield from` 后面需要加的是可迭代对象，它可以是普通的可迭代对象，也可以是迭代器，甚至是生成器。

`yield from Iterable` 可以等价看做 `for item in Iterable: yield item`。
