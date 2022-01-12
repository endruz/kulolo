# Python 编码与解码

- [Python 编码与解码](#python-编码与解码)
  - [str.encode()](#strencode)
  - [bytes.decode()](#bytesdecode)
  - [十六进制 Unicode 编码与汉字的转换](#十六进制-unicode-编码与汉字的转换)
    - [汉字转换十六进制 Unicode 编码](#汉字转换十六进制-unicode-编码)
    - [十六进制 Unicode 编码转换汉字](#十六进制-unicode-编码转换汉字)

字符串通过编码成为字节码，字节码通过解码成为字符串。

Python 中编码和解码是通过 `str.encode()` 和 `bytes.decode()` 来实现。

## [str.encode()](https://docs.python.org/3/library/stdtypes.html?highlight=decode#str.encode)

官方文档中的解释如下：

>str.encode(encoding ="utf-8",errors ="strict")
将字符串的编码版本作为字节对象返回。默认编码是'utf-8'。可以给出错误以设置不同的错误处理方案。错误的默认值是'strict'，意味着编码错误会引发错误UnicodeError。其他可能的值'ignore'，'replace'，'xmlcharrefreplace'， 'backslashreplace'和其他任何名义通过挂号 codecs.register_error()，见错误处理程序。有关可能的编码列表，请参阅标准编码部分。

```python
>>> s = "测试"
>>> type(s)
<class 'str'>
>>> s
'测试'
>>> type(s.encode())
<class 'bytes'>
>>> s.encode()
b'\xe6\xb5\x8b\xe8\xaf\x95'
```

## [bytes.decode()](https://docs.python.org/3/library/stdtypes.html?highlight=decode#bytes.decode)

官方文档中的解释如下：

>bytes.decode(encoding ="utf-8",errors ="strict")
从给定的字节返回字符串解码。默认编码是 'utf-8'。可以给出错误以设置不同的错误处理方案。错误的默认值是'strict'，意味着编码错误会引发错误UnicodeError。其他可能的值是 'ignore'，'replace'以及通过其注册的任何其他名称 codecs.register_error()，请参见错误处理程序部分。有关可能的编码列表，请参阅标准编码部分。

```python
>>> s = b'\xe6\xb5\x8b\xe8\xaf\x95'
>>> type(s)
<class 'bytes'>
>>> s
b'\xe6\xb5\x8b\xe8\xaf\x95'
>>> type(s.decode())
<class 'str'>
>>> s.decode()
'测试'
```

## 十六进制 Unicode 编码与汉字的转换

### 汉字转换十六进制 Unicode 编码

```python
>>> s = "测试"
# 编码为 Unicode 编码
>>> se = s.encode('unicode_escape')
>>> type(se)
<class 'bytes'>
>>> se
b'\\u6d4b\\u8bd5'
# 解码为 UTF-8 编码
>>> sd = se.decode('utf-8')
>>> type(sd)
<class 'str'>
>>> sd
'\\u6d4b\\u8bd5'
```

### 十六进制 Unicode 编码转换汉字

```python
>>> s = '\\u6d4b\\u8bd5'
# 编码为 UTF-8 编码
>>> se = s.encode('utf-8')
>>> type(se)
<class 'bytes'>
>>> se
b'\\u6d4b\\u8bd5'
# 解码为 Unicode 编码
>>> sd = se.decode('unicode_escape')
>>> type(sd)
<class 'str'>
>>> sd
'测试'
```
