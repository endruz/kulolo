# Python 控制结构

- [Python 控制结构](#python-控制结构)
  - [顺序结构](#顺序结构)
  - [选择结构](#选择结构)
    - [if 语句](#if-语句)
    - [if/else 三目运算](#ifelse-三目运算)
  - [循环结构](#循环结构)
    - [while循环](#while循环)
    - [for循环](#for循环)
      - [for/else 语句](#forelse-语句)
    - [循环控制语句](#循环控制语句)

一般编程语言都有三大程序结构：顺序、选择、循坏结构，Python 也不例外。

## 顺序结构

这种是最简单的结构,也是最普遍的结构。若不改变程序执行顺序的话,默认顺序向下执行。

## 选择结构

### if 语句

Python 中 if 语句的一般形式如下所示：

```python
if condition_1:
    statement_block_1
elif condition_2:
    statement_block_2
else:
    statement_block_3
```

注：Python 中用 elif 代替了 else if，所以 if 语句的关键字为：if – elif – else。

### if/else 三目运算

Python 中三目运算形式如下：

```python
a if c else b
```

含义为如果 c 成立，则运行 a ，否则运行 b。即

```python
if sex == 'male':
    text = '男'
else:
    text = '女'
```

等价于

```python
text = '男' if sex == 'male' else '女'
```

注：Python 中并不支持 switch/case 语句。

## 循环结构

### while循环

Python 中 while 语句的一般形式：

```python
while condition:
    statement_block_1
else：
    statement_block_2
```

在 while 循环遍历完（即不是通过 break 跳出而中断的）之后执行 else 的语句块（else 可以省略）。

### for循环

for 循环的语法格式如下：

```python
for iterating_var in sequence:
   statement_block
```

另外一种执行循环的遍历方式是通过索引，如下实例：

```python
>>> L = ['1','2','3']
>>> for index in range(len(L)):
...     print(L[index])
...
1
2
3
>>>
```

#### for/else 语句

for/else 是 Python 中特有的语法格式，else 中的代码在 for 循环遍历完所有元素（即不是通过 break 跳出而中断的）之后执行。

```python
mylist = [1,3,5]
num = 1 # 2
flag = True
for i in mylist:
    if i == num:
        flag = False
        break
if flag:
    print("{} is not in mylist...".format(num))
```

等同于

```python
for i in mylist:
    if i == num:
        break
else:
    print("{} is not in mylist...".format(num))
```

### 循环控制语句

循环控制语句可以更改语句执行的顺序。Python 支持以下循环控制语句：

控制语句 | 描述
:-: | :-
break | 在语句块执行过程中终止循环，并且跳出整个循环
continue | 在语句块执行过程中终止当前循环，跳出该次循环，执行下一次循环。
pass | pass是空语句，是为了保持程序结构的完整性。pass 不做任何事情，一般用做占位语句。
