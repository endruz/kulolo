# Python单例模式实现及特点

- [Python单例模式实现及特点](#python单例模式实现及特点)
  - [一、装饰器](#一装饰器)
    - [线程不安全](#线程不安全)
    - [线程安全](#线程安全)
    - [装饰器单例特点](#装饰器单例特点)
  - [二、`__new__`](#二__new__)
    - [线程不安全](#线程不安全-1)
    - [线程安全](#线程安全-1)
    - [`__new__` 单例特点](#__new__-单例特点)
  - [三、metaclass](#三metaclass)
    - [线程不安全](#线程不安全-2)
    - [线程安全](#线程安全-2)
    - [metaclass 单例特点](#metaclass-单例特点)

单例模式是最常用的一个设计模式，这一模式的目的是使得类的一个对象成为系统中的唯一实例。

Python 中有多种方式来实现单例，下边就介绍这几种方法并分析其特点。

## 一、装饰器

### 线程不安全

```python
from functools import wraps


def singleton(cls):
    # 创建字典用来保存对象，与内层函数 _singlenton 形成闭包
    _instances = {}

    # 不添加装饰器 wraps，单例类的 __name__ 会变成 _singlenton
    @wraps(cls)
    def _singlenton(*args, **kargs):
        # 判断字典中是否含存在该类的对象
        # 若不存在，则创建对象并保存到字典中，然后返回该对象
        # 若存在，则直接返回该对象
        if cls not in _instances:
            _instances[cls] = cls(*args, **kargs)
        return _instances[cls]

    return _singlenton


@singleton
class Singleton:
    def __new__(cls, *args, **kargs):
        # TypeError: super() argument 1 must be type, not function
        # return super(Singleton, cls).__new__(cls)
        return super().__new__(cls)

    def __init__(self, a=0):
        self.a = a
        super().__init__()
```

> PS:  直接使用多线程执行可能会输出正常误以为线程安全，需要适当加入一些 I/O 操作，例如在 `super().__new__(cls)` 前加入 `time.sleep(1)` 来模拟。

### 线程安全

```python
import threading
from functools import wraps


def singleton(cls):
    _instances = {}
    # 创建线程锁，与内层函数 _singlenton 形成闭包
    _instance_lock = threading.Lock()

    @wraps(cls)
    def _singlenton(*args, **kargs):
        # 判断和创建对象部分加锁
        with _instance_lock:
            if cls not in _instances:
                _instances[cls] = cls(*args, **kargs)
        return _instances[cls]

    return _singlenton


@singleton
class Singleton:
    def __new__(cls, *args, **kargs):
        # TypeError: super() argument 1 must be type, not function
        # return super(Singleton, cls).__new__(cls)
        return super().__new__(cls)

    def __init__(self, a=0):
        self.a = a
        super().__init__()
```

### 装饰器单例特点

1. 使用装饰器实际上为 `singleton(Singleton)` 返回的函数 `_singlenton`。所以装饰后的类使用 `type()` 判断的类型都为 function，由于该特性单例类无法直接被继承；
2. 装饰器单例类只会在第一次创建对象时执行 `__init__`，适合只需初始化一次或初始化操作耗时较长的类，例如全局配置类或模型类。

## 二、`__new__`

### 线程不安全

```python
class Singleton:
    def __new__(cls, *args, **kwargs):
        # 构造对象时，判断是否存在类属性 _instance
        # 若不存在，则创建对象并赋值到 _instance 中，然后返回 _instance
        # 若存在，则直接返回 _instance
        if not hasattr(cls, "_instance"):
            cls._instance = super(Singleton, cls).__new__(cls)
        return cls._instance
```

### 线程安全

```python
import threading


class Singleton:
    # 创建线程锁作为类属性
    _instance_lock = threading.Lock()
    def __new__(cls, *args, **kwargs):
        # 判断和创建对象部分加锁
        with cls._instance_lock:
            if not hasattr(cls, "_instance"):
                cls._instance = super(Singleton, cls).__new__(cls)
        return cls._instance
```

### `__new__` 单例特点

1. 因为 `__init__` 在 `__new__` 返回一个对象的时候被调用，所以每次都会进行初始化；
2. 可通过继承使其子类也具备单例的特性。


## 三、metaclass

### 线程不安全

```python

class SingletonMetaClass(type):
    # 创建类属性 _instances 用来保存对象
    _instances = {}
    def __call__(cls, *args, **kwargs):
        # 调用时，判断 _instances 中是否存在该类的对象
        # 若不存在，则正常调用获取对象并添加到 _instances 中，然后返回该对象
        # 若存在，则直接返回该类的对象
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]


class Singleton(metaclass=SingletonMetaClass):
    def __init__(self, a=0):
        self.a = a
        super().__init__()
```

### 线程安全

```python
import threading


class SingletonMetaClass(type):
    _instances = {}
    # 创建线程锁作为类属性
    _instance_lock = threading.Lock()
    def __call__(cls, *args, **kwargs):
        # 判断和创建对象部分加锁
        with cls._instance_lock:
            if cls not in cls._instances:
                cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]


class Singleton(metaclass=SingletonMetaClass):
    def __init__(self, a=0):
        self.a = a
        super().__init__()
```

### metaclass 单例特点

1. metaclass 单例类在调用时只会在第一次创建对象时执行 `__init__`，之后调用直接返回该实例。适合只需初始化一次或初始化操作耗时较长的类，例如全局配置类或模型类。
2. 可通过继承使其子类也具备单例的特性。
