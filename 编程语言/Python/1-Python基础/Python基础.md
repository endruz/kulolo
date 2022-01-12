# Python 基础

- [Python 基础](#python-基础)
  - [注释](#注释)
    - [`#!/usr/bin/env python`](#usrbinenv-python)
    - [`# -*- coding: utf-8 -*-`](#----coding-utf-8---)
  - [标识符](#标识符)
  - [语法格式](#语法格式)
    - [代码块](#代码块)
    - [多行语句](#多行语句)
    - [同一行显示多条语句](#同一行显示多条语句)
  - [变量赋值](#变量赋值)
  - [删除对象](#删除对象)
  - [运行 Python](#运行-python)
    - [Python 交互模式](#python-交互模式)
    - [直接运行 Python 文件](#直接运行-python-文件)

## 注释

Python 中单行注释采用 # 开头。
Python 中多行注释使用三个单引号(''')或三个双引号(""")。

```python
# 单行注释
'''
多行注释1
'''
"""
多行注释2
"""
```

我们通常在文件开头写上这两行注释：

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
```

### `#!/usr/bin/env python`

第一行注释目的就是指出用什么可执行程序去运行代码。
一般 Windows 会忽略这个注释，但是在 Linux/MacOS 必须加上。
以下面的代码为例

```python
def test():
    print 'hello world'

if __name__ == "__main__":
    test()
```

在 Linux 系统中使用`python test.py`执行，会正常输出`hello world`
但若直接运行`./test.py`，会提示

```log
./test.py: line 1: syntax error near unexpected token `('
./test.py: line 1: `def test():'
```

因为系统默认它是 shell 脚本执行了，所以在代码开头加上 `#!/usr/bin/python` 声明使用 Python 解释器来运行，则又成功输出了`hello world`。
第一行的注释有以下两种写法，有着不同含义：

1. `#!/usr/bin/python`

    指定调用 `/usr/bin` 下的 Python 解释器。

2. `#!/usr/bin/env python`(推荐）

    先在 `/usr/bin/env` 中查找到 Python 解释器的安装路径，再调用对应路径下的解释器完成操作。

### `# -*- coding: utf-8 -*-`

第二行注释目的时告诉 Python 解释器，按照 UTF-8 编码**读取**代码，当然你也可以为代码指定 UTF-8 以外的编码格式。
加了第二行注释并不意味着你是用 UTF-8 编码编写的代码，所以**编写**代码时也要确保是使用的 UTF-8 编码。
第二行的注释有以下两种写法：

1. `# -*- coding: UTF-8 -*-`
2. `#coding=utf-8` （注意：`#coding=utf-8` 的 `=` 两边不要空格。）

## 标识符

Python 中的标识符是区分大小写的，标识符由字母、数字、下划线(_)组成，但不能以数字开头，也不能以关键字作为标识符。
Python 的标准库提供了一个 keyword 模块，可以输出当前版本的所有关键字。

```python
>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

## 语法格式

### 代码块

Python 中个人觉得最具特色的就是使用缩进来表示代码块，无需像 Java 一样使用大括号 {} （强迫症的福音！），但是缩进错乱也会导致代码无法运行。

### 多行语句

Python 语句中一般以换行作为语句的结束符。
但是也可以使用斜杠(\\)将一行的语句分为多行显示，如下所示：

```python
total = item_one + \
    item_two + \
    item_three
```

语句中包含 [], {} 或 () 括号就不需要使用多行连接符。如下实例：

```python
days = ['Monday', 'Tuesday', 'Wednesday',
    'Thursday', 'Friday']
```

### 同一行显示多条语句

Python 可以在同一行中使用多条语句，语句之间使用分号(;)分割，以下是一个简单的实例：

```python
>>> s = 'hello world'; print(s)
hello world
```

## 变量赋值

Python 中的变量赋值不需要类型声明。
每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。
Python 可以同时为多个变量赋值。

- 创建一个对象指定多个变量，实际上多个变量都指向同一个对象的内存地址。

    ```python
    >>> a = b = c = 1
    >>> a,b,c
    (1, 1, 1)
    >>> id(a),id(b),id(c)
    (140715621274656, 140715621274656, 140715621274656)
    ```

- 为多个对象指定多个变量。

    ```python
    >>> a, b, c = 1, 2, 3
    >>> a,b,c
    (1, 2, 3)
    ```

注：Python 中一切都是对象，所以上面说的赋值也就是给变量赋予对象。

## 删除对象

给变量赋值时，对象就会被创建。可以使用 del 语句删除变量对对象引用。

```python
>>> a, b, c = 1, 2, 3
>>> del a
>>> a
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'a' is not defined
>>> del b, c
>>> b
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'b' is not defined
>>> c
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'c' is not defined
```

## 运行 Python

运行 Python 有以下两种方式：

### Python 交互模式

进入命令行模式下输入 `python` 命令，就进入了 Python 交互模式，命令提示符是`>>>`。

```powershell
Microsoft Windows [版本 10.0.17134.471]
(c) 2018 Microsoft Corporation。保留所有权利。

C:\>python
Python 3.7.0 (v3.7.0:1bf9cc5093, Jun 27 2018, 04:59:51) [MSC v.1914 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> a = 1
>>> a
1
>>>
```

### 直接运行 Python 文件

在命令行模式下，可以执行 `python filename.py` 直接运行 Python 文件。

`1.py` 路径 `C:\Users\endru\Desktop\1.py`

```python
# -*- coding: utf-8 -*-

path = "file\\1.txt"
f = open(path,'r')
print(f.read())
```

`1.txt` 路径 `C:\Users\endru\Desktop\file\1.txt`

```txt
123456
```

在命令行下运行文件

```powershell
C:\Users\endru\Desktop>python 1.py
123456

C:\Users\endru\Desktop>cd C:\

C:\>python C:\Users\endru\Desktop\1.py
Traceback (most recent call last):
  File "C:\Users\endru\Desktop\1.py", line 4, in <module>
    f = open(path,'r')
FileNotFoundError: [Errno 2] No such file or directory: 'file\\1.txt'
```

当把`1.txt` 路径改为 `C:\file\1.txt`时

```powershell
C:\>python C:\Users\endru\Desktop\1.py
123456

C:\>cd C:\Users\endru\Desktop

C:\Users\endru\Desktop>python 1.py
Traceback (most recent call last):
  File "1.py", line 4, in <module>
    f = open(path,'r')
FileNotFoundError: [Errno 2] No such file or directory: 'file\\1.txt'
```

可见在命令行模式下，`1.py`中的相对路径是相对于命令行路径，而不是`1.py`路径。
