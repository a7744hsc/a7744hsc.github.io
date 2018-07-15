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



## 高级日志教程

logging库采用模块化设计，它包括logger，handler，filter和formatter几部分。

- Loggers 暴露了应用程序直接使用的接口。
- Handlers 负责京日志记录发送到适当的目的地。
- Filter提供了一个过滤输出日志的机制
- Formatters指定了日志记录的输出格式。

日志事件信息会在Loggers，handlers，filter蛇formatters之间以一种LogRecord的格式传递。

日志记录通过调用一个Logger实例的方法来实现。每个实例都有一个名字，名字使用命名空间结构来管理，使用`.`作为分隔符。例如，一个叫做`scan`的logger是`scan.text`，`scan.html`和`scan.pdf`的父对象。Logger名字可以任意设置，他代表日志信息产生的位置。

一个好的实践是使用模块名来对logger进行命名：

```
logger = logging.getLogger(__name__)
```

这意味着logger名字可以表示package/module结构，而且也可以指明日志产生的位置。

logger层级的根是一个叫做root的logger。这也是`debug()`, `info()`, `warning()`, `error()`和`critical()`方法所使用的logger对象。直接调用上述方法和调用root logger的对应方法功能相同，都会在日志中输出root。

我们可以将日志输出到不同的位置。logging库支持将日志写入文件，http请求，基于smtp的邮件，通用sockets，queues或系统特定的日志存储机制（如syslog或者Windows中的event log）。日志目的地通过handler来控制。你可以创建你自己的handler如果内置的handler无法满足你的需求。

默认情况下，不会有默认的日志目的地。可以使用上文中的例子，通过basicConfig指定目的地。当我们调用日志输出方法时（debug等），系统会检查是否有设置目的地，如果没有的话会自动将控制台设置为输出目的地，于此同时也会设置一个默认的格式。

basicConfig的默认格式如下所示：

```
severity:logger name:message
```

格式可以通过先basicConfig方法传递参数来控制。关于格式的更多信息可以看[这里](https://docs.python.org/3/library/logging.html#formatter-objects)

### 日志流

日志事件在logger和handler中的流动可以用下图表示：

![logging flow](/images/logging_flow.png "logging flow")

### Loggers

Logger对象有三个工作。

1. 暴露一些方法给用户，是用户可以通过调用这些方法来记录日志。
2. logger对象根据日志的紧急程度来决定对哪些日志信息进行处理。
3. logger将日志信息传递给相关的log handler。

logger对象最常用的方法分为两大类：配置和发送日志。

有一些非常常见的配置方法：
- Logger.setLevel()指定logger处理的最低紧急程度，debug是最低级别而critical是最高级别。举例来说，如果紧急级别设置为INFO，logger就会处理INFO，WARNING,ERROR和CRITICAL信息并忽略DEBUG信息。
- Logger.addHandler() 和 Logger.removeHandler()。他们用来添加和删除handler对象。Handler会在后文有详细介绍。
- Logger.addFilter()和Logger.removeFilter(),这两个方法用来天剑和删除filter对象。Filter也会在后文进行详细介绍。

通常你不需要对每一个logger调用这些方法，后面会介绍原因。

当有了一个配置好的logger之后，就可以通过调用下面方法来创建log信息了：

- Logger.debug(), Logger.info(), Logger.warning(), Logger.error()和 Logger.critical() 都是用来创建日志记录的，他们所创建的记录等级与其方法名对应。 日志记录的信息是一个格式化字符串，其中可以包括%开头的标准格式化语法（%d，%f，%s等）。后面的参数与格式化字符串中的占位符对应。方法本身只会关注`exc_info`这个命名参数并使用它来确定是否将异常信息写入到日志。

- Logger.exception()创建一个和Logger.error（）相似的日志信息。不同之处在于exception会同时将异常的堆栈信息记录下来，该方法只能在异常处理部分被调用。

- Logger.log()将日志级别作为显式参数传入。这和上面方法比显得冗余，不过使用这个方法可以将日志输出到自定义的等级上。

getLogger（）返回一个特定名称logger的索引，如果是无参调用则返回root logger。多次调用同一方法会返回同一个logger的索引。Logger的名字用点号分割，低层logger是高层logger的后代。例如，`foo.bar`,`foo.bar.baz`都是`foo`的后代。


Logger有一个生效级别的概念，如果未在Logger上显式设置级别，则使用其父级别作为其有效级别。如果父级没有明确的级别设置，则检查其父级，依此类推 - 搜索所有祖先，直到找到显式的级别级别。根Logger始终具有显式级别集（默认情况下为WARNING）。在决定是否处理事件时，Logger的有效级别用于确定事件是否传递给Logger的handler。

子logger将消息传播到与其祖先logger相关联的handlers。因此，不必为应用程序使用的所有logger定义和配置handlers。为顶级logger配置handlers并根据需要创建子logger就足够了。 （但是，您可以通过将logger的`propagate`属性设置为False来关闭传播。）

### Handlers

handlers对象负责将适当的日志消息（基于日志消息的严重性）分派给handlers的目标。 Logger对象可以使用`addHandler`方法向自身添加零个或多个handlers对象。作为示例，应用程序可能希望将所有日志消息发送到日志文件，将错误或更高的所有日志消息发送到标准输出，以及对电子邮件地址至关重要的所有消息。此方案需要三个单独的handlers，其中每个handlers负责将特定严重性的消息发送到特定位置。

标准库包含很多handler类型（请参阅有[用的handler](https://docs.python.org/3/howto/logging.html#useful-handlers)）;这些教程在其示例中主要使用`StreamHandler`和`FileHandler`。

Handler中很少有方法可供应用程序开发人员关注。当使用内置handler对象时（即不创建自定义handler），少有的与应用程序开发人员相关的handler方法是以下配置方法：

- 与logger对象一样，`setLevel()`方法指定分派到目标的最低等级。为什么有两个`setLevel()`方法？logger中设置的级别确定将传递给其handler的消息等级。每个handler中设置的级别确定handler将处理的消息等级。
- `setFormatter()`选择此handler使用的Formatter对象。
- `addFilter()`和`removeFilter()`分别用来添加和移除handlers上的一个Filter对象。

### Formatters

Formatter对象控制日志消息的最终顺序，结构和内容。与基本logging.Handler类不同，应用程序代码可以实例化`formatter`类. 如果应用程序有特殊需求，通过创建一个`formatter`的子类。构造函数有三个可选参数 - 消息格式字符串，日期格式字符串和样式指示符。
```
    logging.Formatter.__init__(fmt=None, datefmt=None, style='%')
```

如果没有消息格式字符串，则默认使用原始消息。如果没有日期格式字符串，则默认日期格式为`%Y-%m-%d %H:%M:%S`. `style`字符可以指定为`%`,`{`或`$`，默认为`%`。

如果使用`%`符号，则消息格式字符串使用`%(<dictionaryker>)s`风格的字符替换，关于`key`的列表在[LogRecord attributes](https://docs.python.org/3/library/logging.html#logrecord-attributes)，`{`代表使用新的与[`str.format()`](https://docs.python.org/3/library/stdtypes.html#str.format)兼容的风格。`$`使用与[`string.Template.substitute()`](https://docs.python.org/3/library/string.html#string.Template.substitute)相同的格式。

注：`style`参数在3.2版本中加入。

下面的格式化字符串会将时间，严重程度和消息打印出来（输出到handler）
```
'%(asctime)s - %(levelname)s - %(message)s'
```

Formatters使用用户可配置的函数将记录的创建时间转换为元组。默认情况下，使用`time.localtime()`;要为特定格式化程序实例更改此值，请将实例的`converter`属性设置为与`time.localtime()`或`time.gmtime()`具有相同签名的函数。要对所有格式化程序进行更改，例如要用GMT时区显示所有记录时间，将`Formatter`类的`converter`属性设置为`time.gmtime`。

## logging的配置

开发者可以使用下面三种方式来配置logging：

1. 使用前文列出的配置方法在Python代码中显式创建logger，handler序和formatter。
2. 创建日志配置文件并使用`fileConfig()`函数读取它。
3. 创建配置信息字典并将其传递给`dictConfig()`函数。

[这里](https://docs.python.org/3/library/logging.config.html#logging-config-api)是最后两种方式的参考文档。以下示例使用Python代码配置一个非常简单的logger，一个控制台handler和一个简单的格式化程序：

```
import logging

# create logger
logger = logging.getLogger('simple_example')
logger.setLevel(logging.DEBUG)

# create console handler and set level to debug
ch = logging.StreamHandler()
ch.setLevel(logging.DEBUG)

# create formatter
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

# add formatter to ch
ch.setFormatter(formatter)

# add ch to logger
logger.addHandler(ch)

# 'application' code
logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')
```

在命令行中运行这个模块得到如下输出:
```
$ python simple_logging_module.py
2005-03-19 15:10:26,618 - simple_example - DEBUG - debug message
2005-03-19 15:10:26,620 - simple_example - INFO - info message
2005-03-19 15:10:26,695 - simple_example - WARNING - warn message
2005-03-19 15:10:26,697 - simple_example - ERROR - error message
2005-03-19 15:10:26,773 - simple_example - CRITICAL - critical message
```

以下Python模块创建的logger，handler和格式化程序与上面列出的示例几乎完全相同，唯一的区别是对象的名称：

```
import logging
import logging.config

logging.config.fileConfig('logging.conf')

# create logger
logger = logging.getLogger('simpleExample')

# 'application' code
logger.debug('debug message')
logger.info('info message')
logger.warn('warn message')
logger.error('error message')
logger.critical('critical message')
```

`logging.conf`:
```
[loggers]
keys=root,simpleExample

[handlers]
keys=consoleHandler

[formatters]
keys=simpleFormatter

[logger_root]
level=DEBUG
handlers=consoleHandler

[logger_simpleExample]
level=DEBUG
handlers=consoleHandler
qualname=simpleExample
propagate=0

[handler_consoleHandler]
class=StreamHandler
level=DEBUG
formatter=simpleFormatter
args=(sys.stdout,)

[formatter_simpleFormatter]
format=%(asctime)s - %(name)s - %(levelname)s - %(message)s
datefmt=
```
输出几乎与基于非配置文件的示例相同:
```
$ python simple_logging_config.py
2005-03-19 15:38:55,977 - simpleExample - DEBUG - debug message
2005-03-19 15:38:55,979 - simpleExample - INFO - info message
2005-03-19 15:38:56,054 - simpleExample - WARNING - warn message
2005-03-19 15:38:56,055 - simpleExample - ERROR - error message
2005-03-19 15:38:56,130 - simpleExample - CRITICAL - critical message
```
可以看到配置文件方法与Python代码方法相比具有一些优势，主要是配置和代码的分离以及非开发人员轻松修改日志配置的能力。


> 警告:`fileConfig()`函数有一个默认参数`disable_existing_loggers`，出于兼容性考虑，其默认为True。它会导致在`fileConfig()`调用之前存在的任何logger被禁用，除非它们（或祖先）在配置中显示配置。如果需要，请将此参数指定为False。
> 传递给`dictConfig()`的字典也可以指定key为`disable_existing_loggers`的布尔值，如果未在字典中指定，则默认值为True。这会导致上面描述的logger禁用行为，如果这不是您想要的，需要显式将其指定为False。

请注意，配置文件中引用的类名称需要存在于logging模块中，或者可以使用常规`import`机制解析。因此，您可以使用WatchedFileHandler（包含于日志记录模块）或mypackage.mymodule.MyHandler（对于在mypackage包和模块mymodule中定义的类，其中mypackage在Python路径中）。

在Python 3.2中，引入了一种新的配置日志记录的方法，使用字典来保存配置信息。这提供了上面的基于配置文件的方法的功能的超集，这是新应用程序和部署的推荐配置方法。因为Python字典用于保存配置信息，并且由于您可以使用不同的方式填充该字典，因此您有更多的配置选项。例如，您可以使用JSON格式的配置文件，或者，如果您有权访问YAML处理功能，则可以使用YAML格式的文件来填充配置字典。或者您可以在Python代码中构建字典，通过套接字以pickle形式接收它，或者使用对您的应用程序有意义的任何方法。

这里是使用YAML格式书写的基于字典的方式，他的配置和上面方法相同：

```
version: 1
formatters:
  simple:
    format: '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
handlers:
  console:
    class: logging.StreamHandler
    level: DEBUG
    formatter: simple
    stream: ext://sys.stdout
loggers:
  simpleExample:
    level: DEBUG
    handlers: [console]
    propagate: no
root:
  level: DEBUG
  handlers: [console]
```

更多关于使用字典对logging进行配置的信息在[这里](https://docs.python.org/3/library/logging.config.html#logging-config-api)。

### 如果没有提供配置信息会发生什么？

如果未提供日志记录配置，则可能出现需要输出日志记录事件但无法找到输出事件的handler的情况。在这些情况下，日志包的行为取决于Python版本。

对于3.2版本以前:
- 如果`logging.raiseExceptions`是`False`(生产模式),事件会以静默方式被删除。
- 如果`logging.raiseExceptions`是`True`(开发模式), `No handlers could be found for logger X.Y.Z` 会被打印出来

对于Python 3.2及以后版本：

- 该事件使用“handler of last resort”输出，他存储在`logging.lastResort`中。此内部handler不与任何logger关联，并且像StreamHandler一样将事件描述消息写入sys.stderr中（因此遵从任何有效的重定向）。没有对消息进行格式化 - 只打印原始事件描述消息。handler的级别设置为WARNING，因此将输出此级别和更高级别的所有事件。

- 如果想让其表现的和3.2版本之前一样，则可以将`logging.lastResort`设置为`None`

### 为一个Library配置logging

在开发使用日志记录的库时，您应该注意记录库如何使用日志记录 - 例如，使用的logger名称。还需要考虑其日志记录配置。如果应用程序不使用日志记录，并且库代码进行日志记录调用，则（如上一节所述）严重性为WARNING和更高的事件将打印到sys.stderr。这被认为是最好的默认行为。

如果由于某种原因您不希望在没有任何日志记录配置的情况下打印这些消息，则可以将无操作handler附加到库的顶级logger。这样可以避免打印消息，因为将始终为库的事件找到handler：它不会产生任何输出。如果库用户配置了logger（并配置了handler）供应用程序使用，则在库代码中进行的日志记录调用将正常地将输出发送给这些handler。

日志库中包含一个do-nothinghandler：`NullHandler`（自Python 3.1起）。可以将此handler的实例添加到库使用的日志记录命名空间的顶级logger中（如果要在没有日志记录配置的情况下阻止将库的记录事件输出到sys.stderr）。如果库foo的所有日志记录都是使用名称匹配'foo.x'，'foo.x.y'等的logger完成的，那么应该使用下面代码：

```
import logging
logging.getLogger('foo').addHandler(logging.NullHandler())
```

如果组织生成了许多库，则指定的logger名称可以是“orgname.foo”而不仅仅是“foo”。

> 注意: 强烈建议您不要将`NullHandler`以外的任何handler添加到库的logger中。因为handler的配置是使用您的库的应用程序开发人员的权利。应用程序开发人员了解他们的目标受众以及哪些handler最适合他们的应用程序：如果您在库内部添加handler，则可能会干扰他们执行单元测试和提供符合其要求的日志的能力。

## Logging 等级
日志记录级别的数值在下表中给出。如果要定义自己的级别，并且于预定义级别同时使用，则需要关注这些级别。如果您使用相同的数值定义级别，它将覆盖预定义的值;预定义的名称丢失。

|Level|数字值|
| - | - |
|CRITICAL|	50|
|ERROR|	40|
|WARNING|	30|
|INFO|	20|
|DEBUG|	10|
|NOTSET|	0|

级别也可以与logger相关联，由开发人员设置或通过加载已保存的日志记录配置来设置。在logger上调用日志记录方法时，logger会将其自身级别与与方法调用关联的级别进行比较。如果logger的级别高于方法调用的级别，则实际上不会生成任何记录消息。这是控制日志输出详细程度的基本机制。

记录消息被编码为LogRecord类的实例。当logger决定记录事件时，将从记录消息创建LogRecord实例。

记录消息通过handler来处理调度机制，handler是Handler类的子类的实例。handler负责确保记录的消息（以LogRecord的形式）最终位于特定位置（或一组位置），这对该消息的目标受众有用（例如最终用户，支持服务台人员，系统管理员，开发人员）。handler传递用于特定目标的LogRecord实例。每个logger可以有零个，一个或多个与之关联的handler（通过Logger的`addHandler()`方法）。除了与logger直接关联的任何handler之外，还调用与logger的所有祖先关联的所有handler来分派消息（除非logger的`propagate`标志设置为false值，此时停止传递给祖先handler）。

就像logger一样，handler可以具有与它们相关联的级别。handler的级别充当过滤器，其方式与logger级别相同。如果handler决定处理事件，则使用`emit()`方法将消息发送到其目标。大多数用户定义的Handler子类都需要覆盖此方法。

###自定义 Level

可以定义您自己的logging级别，但你不一定需要这么做，因为现有级别是根据实践经验选择的。但是，如果您确信需要自定义级别，则应特别小心，如果您正在开发库，则自定义级别可能是一个非常糟糕的主意。这是因为如果多个库作者都定义了他们自己的自定义级别，那么应用程序开发者很难控制和解释这些多个库的日志记录输出，因为对于不同的库，给定的数值可能意味着不同的东西。

## 实用的Handler

除了基础的Handler外，logging还包括下面这些handler，用于以不同形式输出log：

1. [StreamHandler](https://docs.python.org/3/library/logging.handlers.html#logging.StreamHandler), 将消息发送到流（类文件对象）。
2. [FileHandler](https://docs.python.org/3/library/logging.handlers.html#logging.FileHandler), 将消息发送到磁盘文件。
3. [BaseRotatingHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.BaseRotatingHandler)是在循环日志文件的handler的基类。 开发者并不直接实例化该类， 而是使用RotatingFileHandler或TimedRotatingFileHandler 。
4. [RotatingFileHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.RotatingFileHandler),将消息发送到磁盘文件，支持最大日志文件大小和日志文件轮换。
5. [TimedRotatingFileHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.TimedRotatingFileHandler),将消息发送到磁盘文件，以特定的时间间隔更新日志文件。
6. [SocketHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.SocketHandler), 将消息发送到TCP / IP套接字。 从3.4开始，也支持Unix域套接字。
7. [DatagramHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.DatagramHandler), 将消息发送到UDP套接字。 从3.4开始，也支持Unix域套接字。
8. [SMTPHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.SMTPHandler), 将消息发送到指定的电子邮件地址。
9. [SysLogHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.SysLogHandler), 将消息发送到Unix syslog守护程序，可能在远程计算机上。
10. [NTEventLogHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.NTEventLogHandler), 将消息发送到Windows NT / 2000 / XP事件日志。
11. [MemoryHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.MemoryHandler), 将消息发送到内存中的缓冲区，只要满足特定条件，就会刷新。
12. [HTTPHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.HTTPHandler), 使用任一语义GET或POST语义将消息发送到HTTP服务器。
13. [WatchedFileHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.WatchedFileHandler), 会监视他们要登录的文件。如果文件发生更改，则会关闭该文件并使用文件名重新打开。此handler仅在类Unix系统上有用; Windows不支持使用的基础机制。
14. [QueueHandler](https://docs.python.org/3/library/logging.handlers.html#logging.handlers.QueueHandler), 将消息发送到队列，例如在queue或multiprocessing模块中实现的那些队列，该handler在Python3.1即以后版本中提供

15. [NullHandler](https://docs.python.org/3/library/logging.handlers.html#logging.NullHandler), 不执行任何错误消息。一般由想要使用日志记录的库开发人员使用，使用它可以避免显示”No handlers could be found for logger XXX“消息（在库用户没有配置logging时）。更多信息，请参阅[配置库的日志](https://docs.python.org/3/howto/logging.html#library-config)记录。该handler在Python3.1即以后版本中提供。

NullHandler，StreamHandler和FileHandler类在核心logging包中定义。其他handler在子模块logging.handlers中定义。 （还有另一个子模块logging.config，用于配置功能。）

消息通过Formatter类的实例进行格式化。他支持多种格式化方式（Formatter章节有详细介绍）。

对于批量格式化多个消息，可以使用`BufferingFormatter`的实例。除了格式字符串（应用于批处理中的每个消息）之外，还提供了标题和尾部格式字符串。

当基于logger级别和/或handler级别的过滤不够时，可以将Filter的实例添加到Logger和Handler实例（通过他们的`addFilter()`方法）。在决定处理消息之前，logger和handler都会查询其所有过滤器以获取权限。如果任何过滤器返回false值，则不会进一步处理该消息。

基本的过滤器功能允许按特定的logger名称进行过滤。如果使用此功能，则允许通过过滤器发送到指定logger及其子logger的消息，并删除所有其他消息。


## Exceptions raised during logging

> 个人觉得没那么重要，所以没有详细翻译

在production模式下，logging会忽略将自身的报错，防止logging程序将主程序终止，更多内容请参考[原文](https://docs.python.org/3/howto/logging.html#exceptions-raised-during-logging)。

## Using arbitrary objects as messages

可以将任意对象传入 logger，只要其实现了`__str__()`即可。

## 优化


消息参数的格式化将被推迟,直到必须执行时才会真正执行。但是，计算传递给日志记录方法的参数也可能很昂贵，您可能希望不去计算这些参数如果logger不会处理您的事件。要决定做什么，可以调用`isEnabledFor()`方法，该方法接受一个level参数，如果Logger会处理该级别事件，则返回true。你可以写这样的代码来节约资源：

```
if logger.isEnabledFor(logging.DEBUG):
    logger.debug('Message with %s, %s', expensive_func1(),
                                        expensive_func2())

```

> 注意: 在某些情况下，`isEnabledFor()`本身可能比您想的更昂贵（例如，对于深度嵌套的logger，其中显式级别仅在logger高层次结构中设置）。在这种情况下（或者如果你想避免在循环中调用方法），你可以在本地或实例变量中缓存`isEnabledFor()`的调用结果，并且每次都使用它而不是调用方法。只有当日志记录配置在应用程序运行时动态更改（这不常见）时才需要重新计算该缓存。


还可以针对特定应用程序进行其他优化，这些应用程序需要更精确地控制收集的日志记录信息。以下是您可以执行的操作列表，以避免在您不需要的日志记录期间进行处理：

|不需要收集的内容|怎样避免收集它|
|：-|：-|
|Information about where calls were made from.|Set logging._srcfile to None. This avoids calling sys._getframe(), which may help to speed up your code in environments like PyPy (which can’t speed up code that uses sys._getframe()), if and when PyPy supports Python 3.x.|
|Threading information.|Set logging.logThreads to 0.|
|Process information.|Set logging.logProcesses to 0.|


另请，核心logging模块仅包含基本handler。如果不导入logging.handlers和logging.config，它们将不占用任何内存。



## See also
Module [logging](https://docs.python.org/3/library/logging.html#module-logging)
API reference for the logging module.

Module [logging.config](https://docs.python.org/3/library/logging.config.html#module-logging.config)
Configuration API for the logging module.

Module [logging.handlers](https://docs.python.org/3/library/logging.handlers.html#module-logging.handlers)
Useful handlers included with the logging module.

A logging [cookbook](https://docs.python.org/3/howto/logging-cookbook.html#logging-cookbook)