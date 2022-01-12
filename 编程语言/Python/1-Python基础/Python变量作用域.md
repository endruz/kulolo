# 变量作用域

- [变量作用域](#变量作用域)
  - [global 和 nonlocal 关键字](#global-和-nonlocal-关键字)
    - [global](#global)
    - [nonlocal](#nonlocal)

作用域是程序运行时变量可被访问的范围，Python 中作用域主要有以下四种：

1. L (Local) 局部作用域，函数内的作用域。
2. E (Enclosing) 嵌套作用域，外层非全局作用域。
3. G (Global) 全局作用域，全局变量即在当前模块中定义的变量，每一个模块都是一个全局作用域。
4. B (Built-in) 内置作用域，系统内固定模块即内置作用域，内置作用域变量即 Python 内置变量。

(Python 中 `if/elif/else` `try/except` `for` `while`不能改变作用域。)
Python 引用变量的顺序为：局部变量 → 嵌套变量 → 全局变量 → 内置变量。

```python
print(__name__)  # B
a = 1 # G
def outer():
    a = 2 # E
    def inner():
        a = 3 # L
        print(a)
    inner()

outer()
```

上面例子中四个变量，后面注释内容 B, G, E, L 分别对应内置变量、全局变量、嵌套变量、局部变量。

若对这几个拥有相同变量名的变量使用不同的组合方式注释掉，会根据引用变量的顺序来输出不同的结果。

## global 和 nonlocal 关键字

在内部作用域中可以随意读取外部作用域的变量，却无法直接改变这个变量所指向的内存地址（若改变指向的地址，则视为在内部作用域中新建一个变量并赋值）。

```python
>>> g = [1]
>>> def fun():
...     g = [1,2]
...     print("fun g:",g)
...
>>> print("before fun g:",g)
before fun g: [1]
>>> fun()
fun g: [1, 2]
>>> print("after fun g:",g)
after fun g: [1]
```

所以只有当外部作用域的变量是可变数据类型时，我们可以修改其数据。

```python
>>> g = [1]
>>> def fun():
...     g.append(2)
...     print("fun g:",g)
...
>>> print("before fun g:",g)
before fun g: [1]
>>> fun()
fun g: [1, 2]
>>> print("after fun g:",g)
after fun g: [1, 2]
```

在这样我们在内部作用域操作外部作用域的变量时就十分不便。所以 Python 提供了`global` 和 `nonlocal` 这两个关键字用来在内部作用域修改外部作用域的变量。

### global

`global` 关键字用来修改全局变量。

```python
>>> g = [1]
>>> def fun():
...     global g
...     g = [1,2]
...     print("fun g:",g)
...
>>> print("before fun g:",g)
before fun g: [1]
>>> fun()
fun g: [1, 2]
>>> print("after fun g:",g)
after fun g: [1, 2]
```

### nonlocal

`nonlocal` 关键字用来修改嵌套作用域（外层非全局作用域）中的变量。

```python
>>> def outer():
...     def inner():
...             nonlocal n
...             n = "get nonlocal variable"
...             print("inner n:",n)
...     n = "nonlocal variable"
...     print("before inner n:",n)
...     inner()
...     print("after inner n:",n)
...
>>> outer()
before inner n: nonlocal variable
inner n: get nonlocal variable
after inner n: get nonlocal variable
```

若函数嵌套了多层则`nonlocal` 关键字获取的为最近的一个变量。

```python
def outer():
    def inner1():
        # b = ["outer","inner1"]
        def inner2():
            nonlocal b
            b = b + ["inner2"]
            print("inner2 b:",b)
        return inner2
    b = ["outer"]
    print("before b:",b)
    inner1()()
    print("after b:",b)

outer()
```

大家可以想一想第三行是否注释对最终输出结果有何影响。
