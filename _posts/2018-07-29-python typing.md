---
layout: post
title:  "Python新特性 Python typing的使用"
date:   2018-07-29 01:00:00 +0800
categories: Python
---

## Python是一门动态类型语言

大家都知道Python是一门解释型语言，所有的语法分析都是在执行时进行而不需编译。那么Python是强类型语言还是弱类型语言呢？这个问题很多人容易搞混，觉得Python可以给同一个变量赋不同类型的值，把它当做弱类型语言。而事实上Python是强类型语言，动态类型语言。一般来说，强类型指每个（变量引用的）值都有类型的概念，值的行为由其类型决定且类型之间不能隐式转换;而动态类型主要指变量本身的类型是在运行时决定的，在运行过程中它可以指向不同类型的值。对Python来说这很合理，因为Python本就是解释型语言，不会在程序执行前对代码进行检查。动态类型带来的直观表现有两点。第一，在创建变量时无需指定数据类型；第二，可以将不同类型的值赋给同一变量。关于Python类型的信息可以参考下面例子：
```
>>> 'hello'+2018 # 不同类型之间不能直接相加

Traceback (most recent call last):
  File "<input>", line 1, in <module>
TypeError: must be str, not int

>>> 'asdasd'+str(2018)  # 将int类型显示转换成str即可成功与字符串相加
'asdasd2018'

# 可以将不同类型的值赋给变量`a`
a=3
a='1123sd'

```

动态类型无需在创建变量时指定数据类型，通过duck typing可以开发更加通用的方法，快速的调整模型，这些便利可以显著提升小规模程序的开发效率。但是万物都有两面性，当项目变大变复杂的时候，动态类型的缺点就会变得明显，没有完善的代码提示，代码可读性差等缺点会使代码维护变得困难。更详细的关于静态类型和动态类型优缺点的讨论可以再[这里](https://softwareengineering.stackexchange.com/questions/122205/what-is-the-supposed-productivity-gain-of-dynamic-typing)找到。


## Python typing - 提高Python代码可维护性

由于前面介绍的种种原因，大型Python项目代码可读性往往不能与Java等静态类型语言相比。 Python开发者为了解决这种问题使用了各种各样的方法，这其中包括为项目实现大量的测试代码，规范化的代码注释等等。这些方法虽然从一定程度上解决了问题，但仅仅是项目上的约定，并没有形成统一的规范。

为了解决问题，统一规范，Python在3.5版本中引入了[typing 模块](https://docs.python.org/3/library/typing.html)，实现了类型提示功能。需要注意的是，目前该模块依然处于孵化阶段，这意味着该模块的接口可能会出现变更。

typing模块的使用非常简单，你可以像其他语言一样在声明变量时指定变量类型，创建方法时指定参数类型，通过下面几个例子可以直观的理解其用法：
```
# 一般参数
def greeting(name: str) -> str:
    return 'Hello ' + name

# 可执行参数
from typing import Callable
def feeder(get_next_item: Callable[[], str]) -> None:
    # Body

# 容器类型
from typing import Mapping, Sequence
def notify_by_email(employees: Sequence[Employee],
                    overrides: Mapping[str, str]) -> None:

# 泛型
from typing import Sequence, TypeVar
T = TypeVar('T')      # Declare type variable
def first(l: Sequence[T]) -> T:   # Generic function
    return l[0]
```

> 这里需要注意的一点是，Python typing本质是一种注释，可以用来帮助IDE进行类型检查和自动补全等操作，但是他并不会真的让执行失败。

使用Python typing的好处：
- 实现部分静态类型检查，可以在执行前找到类型错误。 
- 让代码具有更好的自解释性，Clean code推荐少写注释，TW也崇尚不写注释。但是在Python的上下文中注释是提高代码可解释性非常重要的手段，有了类型修饰符，可以部分解决这个问题（自解释的代码总是好于注释的，因为注释很容易被放弃维护）。
- IDE可以提供更精准的代码提示，提高开发效率。

## Pycharm 中使用Python typing

前面说过Python typing是一种annotation，能够帮助IDE进行类型检测和自动补全提示。作为Python IDE的佼佼者，在最新版本的Pycharm中已经支持该特性并默认开启（不过默认类型错误严重等级为Warning，我个人将它修改为error）。目前来看，该功能的总体表现还是不错的，可用性很好。下面是使用Pycharm开发时使用类型检测的效果图（这里我已经自己将严重等级调整为error，默认为warning）
![Pycharm 类型报警](../images/python-typing-error.png?style=center)

自动补全效果图：
![Pycharm 自动补全效果图](../images/python-typing-autocomplementation.png?style=center)


## Python typing 与 duck type

Python typing帮助我们明确标识出方法参数的类型，这带来了很多好处。但是Python typing本身和Python的哲学存在一定冲突，其中以duck type最为明显。在Python中，并不强调对象类型，而更加强调对象行为，“如果他像鸭子一样走路并且像鸭子一样叫，他就是是鸭子”。这种思想帮助我们在Python中实现了很多可以用于不同类型的方法，如`len()`方法可以用于多种数据类型:
```
a= ('a',3)
b=[1,2,3]
c = 'ads'

>>> print(len(a))
2
>>> print(len(b))
3
>>> print(len(c))
3
```

Python typing可以通过[Abstract Base Classes](https://docs.python.org/3/library/abc.html)来支持这种设计。使用ABC可以生成各种各样的抽象类，这些类中包含了一些方法的签名。使用这些抽象类作为参数类型就可以实现通用方法。不过该方法并不是一个官方解决方案，正式的解决方案还在[讨论中](https://github.com/python/typing/issues/11)。

## 总结

Python typing特性还处在开发和完善阶段，所以有些地方（如上面提到的duck typing）还有所欠缺，在生产项目上使用该特性还有一定风险。但是通过我这一段时间的适用，并没有遇到难以解决的问题，而且该特性给我的开发带来了很大的帮助，也使代码易读性大大提高。所以我认为现在已经是开始学习和使用该技术的好时机（现在看来即使未来api改动也不会带来太大负面影响）。