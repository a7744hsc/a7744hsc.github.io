---
layout: post
title:  "使用 Python logging模块进行日志收集（翻译）"
date:   2018-06-16 01:00:00 +0800
categories: Python
---


本文是[Logging HOWTO](https://docs.python.org/3/howto/logging.html)第一部分的翻译，介绍了如何使用logging及如何使用basicConfig来配置logging模块

-------------------------------------

日志收集是用来检测软件执行是发生的事件。开发者在代码中加入日志代码来表明某个事件被触发。事件由文本标书，其中可以包含一些变量信息。每一个事件都有一个重要程度描述，也被叫做日志等级。

何时使用 `logging`

`logging` 模块提供了一些列简便的方法来简化模块的使用。这些方法是 `debug()`,`info()`,`warning()`,`error()`和`critical()`. 下面表格可以帮助你判断哪种情况下使用哪种模块：


|任务|工具|
|-|-|
|为命令行程序在console中显示输出|print()|
|报告程序中的正常操作（用于状态监控和错误调试）|logging.info() (如果需要详细诊断信息，使用 logging.debug())|
|产生特定运行时事件的劲爆|在函数库中可以使用warning.warn()来报告一个可修复的问题，使用logging.warning()来报告一个不可修复但应该注意的状态|
|报告一个运行时事件的错误|跑出异常（exception）|
|报告被系统处理的异常而不抛出它|logging.error(),logging.exception()和logging.critical()|

Logging方法由其追踪事件的严重程度来命名，下表中有详细描述（按严重程度升序）

|等级|使用时机|
|-|-|
|DEBUG|需要诊断错误的详细信息|
|INFO|确认程序按照预期执行|
|WARNING|代表发生了某些超出预期的事件或者某些问题将要发生|
|ERROR|比较严重的问题，导致系统无法处理某些信息|
|CRITICAL|严重错误，程序无法继续运行|

logging模块默认日志级别是WARNING，所以默认情况下只有WARNING及以上的日志信息会被追踪。

日志可以通过多种方式处理，包括最简单的打印到控制台和写入到磁盘文件。

一个简单地例子

```
import logging
logging.warning('Watch out!')  # will print a message to the console
logging.info('I told you so')  # will not print anything
```

将上述命令保存为脚本并运行可以得到下面的输出：

```
WARNING:root:Watch out!WARNING:root:Watch out!
```

由于默认logging中的level是`info`信息不会显示。打印出来的信息包含日志的级别和日志的描述信息（logging调用时填写的描述），日志的格式可以很灵活，而且可以自定义。

### 输出日志到文件中

一个常见的情况是记录日志文件到磁盘文件中区。请在一个新启动的Python解释器中执行下面代码。

```
import logging
logging.basicConfig(filename='example.log',level=logging.DEBUG)
logging.debug('This message should go to the log file')
logging.info('So should this')
logging.warning('And this, too')
```

执行完毕后如果我们打开文件，则可以看到下面的信息。

```
    DEBUG:root:This message should go to the log file
    INFO:root:So should this
    WARNING:root:And this, too
```

这个例子也战术了怎样市值日志追踪级别。在这个例子中因为我们将日志级别设为DEBUG，所以所有信息都会被打印出来。

如果你希望通过通过`--log=INFO`这样的方式来设置日志等级，而且该参数被存储在叫做`loglevel`的变量中，则可以使用`getattr(logging,loglevel.upper())`方法。

这里可以加入更多的的检查以确保输入到basicConfig（）的参数是正确的，下面是一种可能的例子：
```
# assuming loglevel is bound to the string value obtained from the
# command line argument. Convert to upper case to allow the user to
# specify --log=DEBUG or --log=debug
numeric_level = getattr(logging, loglevel.upper(), None)
if not isinstance(numeric_level, int):
    raise ValueError('Invalid log level: %s' % loglevel)
logging.basicConfig(level=numeric_level, ...)
```

需要在调用日志输出方法之前调用`basicConfig（）`方法以确保配置被应用到后续日志代码中。`basicConfig`方法是一个简易的logging配置工具，他只在第一次执行时生效，第二次和之后的调用不会产生任何操作。

如果你多次执行上面的脚本，日志信息会以追加的形式加入到example.log中，如果你想每次都清空现有信息则可以执行filemode参数，上述例子可以被修改为

```
logging.basicConfig(filename='example.log', filemode='w', level=logging.DEBUG)
```

这段代码的输出和前面的相同，但是日志文件写入方式会从追加编程替换，每次新的运行会清空之前的日志。

### 在多个模块中输出日志

如果你的程序包含多个模块，这里有一个组织logging的例子:

```
# myapp.py
import logging
import mylib

def main():
    logging.basicConfig(filename='myapp.log', level=logging.INFO)
    logging.info('Started')
    mylib.do_something()
    logging.info('Finished')

if __name__ == '__main__':
    main()
```

```
# mylib.py
import logging

def do_something():
    logging.info('Doing something')
```

执行`myapp.py`后，你应该在myqpp.log中看到如下内容：

```
INFO:root:Started
INFO:root:Doing something
INFO:root:Finished
```

这种方法生成的日志并不包含模块信息，所以只能通过日志内容确定日志产生的位置，如果想要追踪日志产生的位置，可以看另一篇[高级日志教程](https://docs.python.org/3/howto/logging.html#logging-advanced-tutorial)

### 记录变量信息

记录变量信息可以使用格式化字符串，如下所示：

```
import logging
logging.warning('%s before you %s', 'Look', 'leap!')
```
这段代码会生成下面的日志：

```
WARNING:root:Look before you leap!
```

如你所见，我们可以使用%风格的格式化字符来合并变量。使用这种老式风格是为了兼容性考虑。logging其他心事格式化选项，详情见(在你的应用中使用特定格式化风哈)[https://docs.python.org/3/howto/logging-cookbook.html#formatting-styles]

### 改变信息格式

可以通过指定格式来空时日志的格式：

```
import logging
logging.basicConfig(format='%(levelname)s:%(message)s', level=logging.DEBUG)
logging.debug('This message should appear on the console')
logging.info('So should this')
logging.warning('And this, too')
```

这会产生如下输出：
```
DEBUG:This message should appear on the console
INFO:So should this
WARNING:And this, too
```

可以发现前面日志中的‘roo’不见了。可以通过[log属性](https://docs.python.org/3/library/logging.html#logrecord-attributes)来查看日志格式中所支持的所有属性。一般我们只需要levelname，message和时间信息，如何输出时间信息将在下节介绍。

### 显示日志的时间信息

可以通过将`%(asctime)s`加入到格式字符转的形式来显示时间：

```
import logging
logging.basicConfig(format='%(asctime)s %(message)s')
logging.warning('is when this event was logged.')
```

这会输出类似下面的信息：

```
2010-12-12 11:41:42,612 is when this event was logged.
```

时间默认会议ISO8601/RFC3339的格式显示，如果你需要更多定制化，可以通过传递一个datefmt参数给basicConfig来实现，示例如下：

```
import logging
logging.basicConfig(format='%(asctime)s %(message)s', datefmt='%m/%d/%Y %I:%M:%S %p')
logging.warning('is when this event was logged.')
```
显示结果：

```
12/12/2010 11:46:36 AM is when this event was logged.
```
这里datefmt的格式和`time.strftime()`所支持的格式相同。

### 下一步：
上面的内容足够你开始使用logging模块。不过logging模块所提供的功能远不止这些，想要进一步了解logging模块，可以[继续阅读](https://docs.python.org/3/howto/logging.html#logging-advanced-tutorial).

如果你的日志需求比较简单，只需将上面例子应用到你的系统中。如果你发现任何问题，可以把他们发送到[这里](https://groups.google.com/group/comp.lang.python)




