# FastAPI 全链路日志追踪

- [FastAPI 全链路日志追踪](#fastapi-全链路日志追踪)
  - [一、背景](#一背景)
  - [二、解决思路](#二解决思路)
  - [三、方案实现](#三方案实现)
    - [1. 协程的上下文中进行标识](#1-协程的上下文中进行标识)
    - [2. 自定义 FastAPI 中间件](#2-自定义-fastapi-中间件)
    - [3. 修改日志模板](#3-修改日志模板)
  - [四、后期优化](#四后期优化)

## 一、背景

在微服务中，查看日志是排查问题的主要方法。但是在并发量大的情况下，使用日志定位问题还是较为麻烦的。由于大量的不同用户、不同进程/线程/协程的日志都混杂在一起输出，导致很难筛选出指定请求产生的相关日志，只能根据请求时间来慢慢排查。

## 二、解决思路

1. 将服务中每个请求都用一个唯一标识来进行标记，以追踪该请求相关的所有日志。
2. 在不修改原有日志打印方式的情况下（无代码入侵），自定义 logging 的 formatter 和 filter，在日志中加入 traceid 标识。

## 三、方案实现

### 1. 协程的上下文中进行标识

Flask 中的 request 是基于一个自定义的 Local 类（类似于 ThreadLocal）实现的。每个访问 Flask 的请求，会绑定到当前的 Context，等请求结束后再销毁。只需要在最初接收到请求时，在 request 中定义唯一标识就可以很方便的实现请求的标记。

由于 FastAPI 的 request 并未实现类似的 Context 功能，决定参考 Flask 的实现方式，在 FastAPI 中实现请求的标记。因为 FastAPI 中各种协程的使用，这边考虑使用 contextvars 进行上下文变量的操作。

contextvars 是 Python 3.7 新加入的一个标准库，用于管理、存储和访问上下文相关的状态，且在 asyncio 中有原生的支持并且无需任何额外配置即可被使用。

> **重要：** contextvars 上下文变量应该在顶级模块中创建，且永远不要在闭包中创建。 Context 对象拥有对上下文变量的强引用，这可以让上下文变量被垃圾收集器正确回收。

```python
import contextvars
request_id_context = contextvars.ContextVar('request-id')
```

### 2. 自定义 FastAPI 中间件

FastAPI 中间件是一个在 request 处理前和 response 返回前被调用的函数。其处理逻辑如下：

1. 接收 request；
2. 执行 request 处理前的自定义逻辑；
3. 传递 request 给应用程序继续处理；
4. 接收应用所产生的 response；
5. 执行 response 返回前的自定义逻辑；
6. 返回 response。

在步骤 2 中设置上下文变量 request-id 在当前上下文中的值，在步骤 5 中清理当前上下文中的上下文变量。

保证该中间件最后一个添加，即可使该中间件第一个接收 request，最后一个返回 response。这样即可保证每个请求从开始到结束存在唯一标识。

```python
import uuid
from fastapi import Request

from app.configs import request_id_context


async def add_request_id_header(request: Request, call_next):
    request_id = str(uuid.uuid4())
    request_id_context.set(request_id)
    response = await call_next(request)
    response.headers["X-Request-Id"] = request_id
    request_id_context.set(None)
    return response
```

### 3. 修改日志模板

日志 formatter 中添加 traceid 关键字，用来在每条输出的日志中显示请求的唯一标识。

```yaml
formatters:
    trace:
        format: "%(asctime)s %(filename)s[line:%(lineno)d] [%(levelname)s] [%(traceid)s]: %(message)s"
        datefmt: '%Y-%m-%d %H:%M:%S'
```

自定义 filter，若该条日志输出所在上下文中存在上下文变量 request-id，则作为 traceid 在日志中输出，否则输出字符串 "TRACE_ID"。

```python
from logging import Filter

from app.configs import request_id_context


class TraceIDFilter(Filter):
    def filter(self, record):
        record.traceid = request_id_context.get("TRACE_ID")
        return True
```

## 四、后期优化

1. 当前方案为单个服务的日志追踪，后期多个服务之间的调用追踪待后续开发；
2. 当前方案为追踪某个请求的日志，后期若有追踪 IP/用户的需求，可使用相同思路进行扩展。
