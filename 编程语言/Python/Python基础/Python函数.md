# Python 函数

- [Python 函数](#python-函数)
  - [参数](#参数)
    - [必需参数](#必需参数)
    - [默认参数](#默认参数)
    - [不定长参数](#不定长参数)
    - [mutable 与 immutable 类型](#mutable-与-immutable-类型)
  - [return](#return)
  - [函数即对象](#函数即对象)
    - [函数的属性](#函数的属性)
    - [函数中定义函数](#函数中定义函数)
    - [函数赋值给变量](#函数赋值给变量)
    - [函数存储到容器对象中](#函数存储到容器对象中)
    - [函数作为参数](#函数作为参数)
    - [函数作为返回值](#函数作为返回值)
  - [函数不记录状态](#函数不记录状态)
  - [匿名函数](#匿名函数)

Python 定义函数的语法：

```python
def functionname( parameters ):
   '''函数说明'''
   function_block
   return [expression]
```

## 参数

### 必需参数

必需参数须以正确的顺序传入函数。调用时的数量必须和声明时的一样。

```python
>>> def fun(name,age):
...     print("name is ",name)
...     print("age is ",age)
...
>>> fun("endRuz", 21)
name is  endRuz
age is  21
```

注：在调用参数时指定参数并赋值，参数的顺序与声明时不一致。

```python
>>> def fun(name,age):
...     print("name is ",name)
...     print("age is ",age)
...
>>> fun(age=21, name="endRuz")
name is  endRuz
age is  21
```

### 默认参数

定义参数时，可以给参数设定一个默认值；调用函数时，如果没有传递参数，则会使用默认参数。

```python
>>> def fun(name,age=18):
...     print("name is ",name)
...     print("age is ",age)
...
>>> fun("endRuz")
name is  endRuz
age is  18
>>> fun("endRuz", 21)
name is  endRuz
age is  21
```

注：必需参数必须在默认参数前，否则 Python 的解释器会报错。
mutable 类型对象不推荐作为函数的默认参数，原因在下面“函数的属性”部分中说明。

### 不定长参数

不定长参数就是传入的参数个数是可变的，语法如下：

```python
def functionname( [parameters,] *[*]variable_parameter ):
   '''函数说明'''
   function_block
   return [expression]
```

加了星号 * 的参数会以元组的形式导入，存放所有未命名的变量参数。若未传参则为一个空元组。

```python
>>> def fun(name,age,*other):
...     print("name is ",name)
...     print("age is ",age)
...     print("other ",other)
...
>>> fun("endRuz", 21, 185, 80)
name is  endRuz
age is  21
other  (185, 80)
>>> fun("endRuz", 21)
name is  endRuz
age is  21
other  ()
```

加了两个星号 ** 的参数会以字典的形式导入。若未传参则为一个空字典。

```python
>>> def fun(name, age, **other):
...     print("name is ",name)
...     print("age is ",age)
...     print(other)
...
>>> fun("endRuz", 21, sex="male", height=185)
name is  endRuz
age is  21
{'sex': 'male', 'height': 185}
>>> fun("endRuz", 21)
name is  endRuz
age is  21
{}
```

若已有一个完整元组或者字典想作为不定长参数传入，只要在调用参数时在参数前加上`*`或者`**`即可。

```python
>>> def fun(name, age, **other):
...     print("name is ",name)
...     print("age is ",age)
...     print(other)
...
>>> d = {"sex":"male", "height":185}
>>> fun("endRuz", 21, **d)
name is  endRuz
age is  21
{'sex': 'male', 'height': 185}
```

注：若定义参数时不定参数后还有参数，调用时需要指定参数并赋值。

一般情况下我们习惯使用 `*args` 和 `**kwargs` 两个参数名来指代不定参数。

### mutable 与 immutable 类型

- immutable 类型作为参数时类似 C++ 的值传递。在函数内部修改参数的值，只是修改另一个复制的对象，不会影响传入的变量本身。
- mutable 类型作为参数时类似 C++ 的引用传递。将参数传入后，在函数内部修改，外部变量也会被修改。

```python
>>> def fun1(a):
...     a+=1
...
>>> b = 3
>>> fun1(b)
>>> b
3
>>> def fun2(c):
...     c.append("over")
...
>>> d = [1,2,3]
>>> fun2(d)
>>> d
[1, 2, 3, 'over']
```

注：在函数内赋值并不会改变传入对象内容，是改变变量的指向的对象。

```python
>>> def fun3(e):
...     print('传入时id',id(e))
...     e = [var*2 for var in e]
...     print('赋值后的值',e)
...     print('赋值后的id',id(e))
...     print('over')
...
>>> f = [1,2,3]
>>> id(f)
2825758073480
>>> fun3(f)
传入时id 2825758073480
赋值后的值 [2, 4, 6]
赋值后的id 2825758570824
over
>>> id(f)
2825758073480
```

## return

return 语句用于退出函数，并返回值。
return 后不带值则返回 None。
Python 中 return 可以同时返回多个值（实际上是一个元组）。

```python
>>> def fun(a,b=2):
...     if a > 0:
...         return a
...     elif a == 0:
...         return
...     else:
...         return a,b
...
>>> fun(1)
1
>>>
>>> str(fun(0))
'None'
>>>
>>> x, y = fun(-1)
>>> x
-1
>>> y
2
>>> fun(-1)
(-1, 2)
```

## 函数即对象

在 Python 中一切皆为对象，函数也不例外。
def 是一条可执行语句，Python 解释器执行 def 语句时，就会在内存中就创建了一个函数对象。函数和其他对象一样，可以做到以下几点：

1. 有自己的属性
2. 有自己的函数
3. 赋值给一个变量
4. 作为元素添加到容器对象中
5. 作为参数值传递给其它函数
6. 作为函数的返回值

### 函数的属性

比如 `__name__` 属性为函数名，`__defaults__`为函数的默认参数列表。

```python
>>> def fun(a=1,b="python"):
...     pass
...
>>> fun.__name__
'fun'
>>> fun.__defaults__
(1, 'python')
```

上面提及过不推荐使用 mutable 类型对象作为默认参数，是因为函数的默认参数是作为属性存储到函数中的，mutable 类型对象上一次调用所受到的影响，在下一次调用时会保留。

```python
>>> def fun(a=1,b=[]):
...     b.append(a)
...     return b
...
>>> fun()
[1]
>>> fun()
[1, 1]
>>> fun()
[1, 1, 1]
```

### 函数中定义函数

```python
>>> def fun():
...     def fun1():
...         print("this is fun1()")
...     print("fun() start")
...     fun1()
...     print("fun() end")
...
>>> fun()
fun() start
this is fun1()
fun() end
>>>
>>> fun1()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'fun1' is not defined
```

在函数内部定义的函数只能在该函数内部使用，在函数之外无法访问。

### 函数赋值给变量

赋值给一个变量时，函数并不会被调用，仅仅是多了一个变量指向了函数对象，变量的使用和函数一致。

```python
>>> def fun(a):
...     print("fun print",a)
...
>>> f = fun
>>> f is fun
True
>>> f("Python")
fun print Python
```

### 函数存储到容器对象中

```python
>>> def fun(a):
...     return a
...
>>> l = ["python",123,fun]
>>> l
['python', 123, <function fun at 0x000002277DD22D08>]
>>> l[2](3.14)
3.14
```

### 函数作为参数

```python
>>> def fun(a):
...     return a+8
...
>>> def fun1(a):
...     b = a(10)
...     return b
...
>>> fun1(fun)
18
```

### 函数作为返回值

```python
>>> def fun(a):
...     return a+8
...
>>> def fun1():
...     return fun
...
>>> fun1()
<function fun at 0x000002277DD61158>
>>> fun1()(10)
18
```

## 函数不记录状态

曾经看过这样一段代码，觉得可以帮助对函数这一部分的理解。

```python
s = []
for x in range(3):
    def fun():
        return x**2
    s.append(fun)
```

可以猜一下列表`s`中每个函数的运行结果是什么？
如果是 `1,4` 那么你就错了，正确答案为 `4,4`。
`fun()`的定义和整块代码其实属于同一作用域，上面的代码也可以写成：

```python
def fun():
   return x**2
s = []
for x in range(3):
   s.append(fun)
```

这样代码可能会更利于理解。
函数对象并没有立刻执行也没有记录`x`的状态，直到被调用了函数才会执行，这时候循环已经结束，`x`保存着最后一个循环结果。

## 匿名函数

Python 使用 lambda 来创建匿名函数。
lambda 函数的语法只包含一个语句，如下：

```python
lambda [arg1 [,arg2,.....argn]]:expression
```

lambda 只是一个表达式，不用写 return，返回值就是该表达式的结果。
匿名函数也是一个函数对象，匿名函数没有名字，不必担心函数名冲突。

```python
>>> sum = lambda x: x**2
>>> sum(3)
9
>>> sum.__name__
'<lambda>'
```
