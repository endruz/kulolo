# Python 多进程场景下的 logging 模块

- [Python 多进程场景下的 logging 模块](#python-多进程场景下的-logging-模块)
  - [一、背景](#一背景)
  - [二、问题分析](#二问题分析)
  - [三、解决思路](#三解决思路)
  - [四、方案实现](#四方案实现)

## 一、背景

logging 是 Python 中做日志记录的一个线程安全的标准库，即在多线程场景下可以保证同一时间只有一个线程执行日志的写操作。

我们在部署 Web 服务时，通常会以多进程的方式启动。这时会发现 logging 模块在多进程场景下并不安全，会造成日志错乱、丢失等种种问题。

## 二、问题分析

以按时间切割日志的场景为例，对 `TimedRotatingFileHandler` 类在多进程场景下的问题进行分析。

`TimedRotatingFileHandler` 继承 `BaseRotatingHandler`，以一定的时间间隔将日志进行切割。当 backupCount>0 时，每次切割完毕后会将超出 backupCount 的旧文件进行删除。

其实现的方法功能如下：
- `computeRollover()`：根据指定的时间算出下次切割的时间；
- `shouldRollover()`：判断是否进行切割操作；
- `getFilesToDelete()`：获取切割后需要删除的文件；
- `doRollover()`：执行日志切割操作。

可以发现其日志切割的主要操作是在 `doRollover()` 中完成的，先分析其操作逻辑。

以下是 `doRollover()` 源码，其中中文注释为其操作逻辑的说明。

```python
def doRollover(self):
    """
    do a rollover; in this case, a date/time stamp is appended to the filename
    when the rollover happens.  However, you want the file to be named for the
    start of the interval, not the current time.  If there is a backup count,
    then we have to get a list of matching filenames, sort them and remove
    the one with the oldest suffix.
    """
    # 切割文件前关闭当前文件流
    if self.stream:
        self.stream.close()
        self.stream = None
    # 计算切割后文件的时间后缀
    # get the time that this sequence started at and make it a TimeTuple
    currentTime = int(time.time())
    dstNow = time.localtime(currentTime)[-1]
    t = self.rolloverAt - self.interval
    if self.utc:
        timeTuple = time.gmtime(t)
    else:
        timeTuple = time.localtime(t)
        dstThen = timeTuple[-1]
        if dstNow != dstThen:
            if dstNow:
                addend = 3600
            else:
                addend = -3600
            timeTuple = time.localtime(t + addend)
    # 切割后的文件名
    dfn = self.rotation_filename(self.baseFilename + "." +
                                    time.strftime(self.suffix, timeTuple))
    # 判断 dfn 文件是否存在，若存在则删除
    if os.path.exists(dfn):
        os.remove(dfn)
    # 切割文件，self.rotate() 继承自 BaseRotatingHandler，其操作为将文件 self.baseFilename 重命名为 dfn
    self.rotate(self.baseFilename, dfn)
    # 删除超出 backupCount 的旧文件
    if self.backupCount > 0:
        for s in self.getFilesToDelete():
            os.remove(s)
    # 打开文件流，self._open() 继承自 FileHandler，打开的文件为 self.baseFilename
    if not self.delay:
        self.stream = self._open()
    # 计算下次切割的时间
    newRolloverAt = self.computeRollover(currentTime)
    while newRolloverAt <= currentTime:
        newRolloverAt = newRolloverAt + self.interval
    #If DST changes and midnight or weekly rollover, adjust for this.
    if (self.when == 'MIDNIGHT' or self.when.startswith('W')) and not self.utc:
        dstAtRollover = time.localtime(newRolloverAt)[-1]
        if dstNow != dstAtRollover:
            if not dstNow:  # DST kicks in before next rollover, so we need to deduct an hour
                addend = -3600
            else:           # DST bows out before next rollover, so we need to add an hour
                addend = 3600
            newRolloverAt += addend
    self.rolloverAt = newRolloverAt
```

简化一下 `TimedRotatingFileHandler` 文件切割的逻辑为：

1. 判断准备切割的文件 dfn 是否存在，如果存在，则删除该文件
2. 然后把 baseFilename 重命名为 dfn
3. 重新打开 baseFilename（新建）文件流

当前逻辑在多进程下，不同进程执行 `doRollover()` 时间不同，会导致上一个进程刚切割出 dfn 在 baseFilename 中写入日之后，下一个进程把判断 dfn 存在将其删除重新切割从而导致日志丢失。

## 三、解决思路

已知 `doRollover()` 删除已存在 dfn 的操作直接导致了日志的丢失，我们可以避免该操作来防止日志丢失。

最后切割的操作逻辑修改为：

- 若 dfn 不存在，则执行切割操作将 baseFilename 重命名为 dfn，重新打开 baseFilename（新建）文件流；
- 若 dfn 已存在，则表示已经有其他进程进行切割了，只需重新打开 baseFilename 文件流。

当其他进程还在写日志时，一个进程进行切割操作并不会影响其他进程日志的输出，其他进程的日志最终都会输出在重命名后的 dfn 文件中。

## 四、方案实现

自定义一个 `MultiProcessSafeTimedRotatingFileHandler` 类继承 `TimedRotatingFileHandler`，重写其 `doRollover()` 方法。

将 `doRollover()` 中的

```python
if os.path.exists(dfn):
    os.remove(dfn)
self.rotate(self.baseFilename, dfn)
```

重写为

```python
if not os.path.exists(dfn):
    try:
        self.rotate(self.baseFilename, dfn)
    except FileNotFoundError:
        pass
```

其中，`try/except` 操作捕获 `FileNotFoundError` 是为了防止其他进程先一步完成了重命名。

