---
layout: post
title:  "Python(python 3)的内置方法-下"
date:   2018-05-30 11:00:00 +0800
categories: Python
---

接[前一篇](https://a7744hsc.github.io/python/2018/05/24/python-builtins.html)，我们介绍一些Python的内置函数

1. frozenset(iterable)

frozenset是一种不可变得集合，与普通的set不同，它在创建后是不可更改的。fronzenset可以计算hash值，所以他可以用来做字典的Key。
```
>>> frozen_set = frozenset([1,2,3])
>>> frozen_set.add(4)
Traceback (most recent call last):
  File "<input>", line 1, in <module>
AttributeError: 'frozenset' object has no attribute 'add'

>>> normal_set = set([1,2,3])
>>> normal_set.add(4)
>>> normal_set
{1, 2, 3, 4}

```


12. globals()，locals()， vars([object]

- globals()返回当前环境下的全局符号表（包括变量和方法）
- locals()返回局部符号表（包括变量和方法）
- vars([object]) 如果不传参数和locals()作用相同，如果传入对象，则返回对象的`__dict__`属性

```
>>> def test_local():
>>>     a=1
>>>     print(locals()) #打印局部变量

>>>test_local()
{'a': 1}

globals() # 打印当前scope的全局变量

{'In': ['',
  'def test_local():\n        a=1\n        print(locals())\n    ',
  'test_local()',
  'global',
  'global()',
  'globals()',
  'global()',
  'global(a)',
  'globals()'],
 'Out': {5: {...}},
 '_': {...},
 '_5': {...},
 '__': '',
 '___': '',
 '__builtin__': <module 'builtins' (built-in)>,
 '__builtins__': <module 'builtins' (built-in)>,
 '__doc__': 'Automatically created module for IPython interactive environment',
 '__loader__': None,
 '__name__': '__main__',
 '__package__': None,
 '__spec__': None,
 .........


 >>> vars(list)
 mappingproxy({'__ge__': <slot wrapper '__ge__' of 'list' objects>, '__iter__': <slot wrapper '__iter__' of 'list' objects>, '__gt__': <slot wrapper '__gt__' of 'list' objects>, 'copy': <method 'copy' of 'list' objects>, 'remove': <method 'remove' of 'list' objects>, '__mul__': <slot wrapper '__mul__' of 'list' objects>, '__setitem__': <slot wrapper '__setitem__' of 'list' objects>, '__rmul__': <slot wrapper '__rmul__' of 'list' objects>, 'clear': <method 'clear' of 'list' objects>, '__imul__': <slot wrapper '__imul__' of 'list' objects>, '__contains__': <slot wrapper '__contains__' of 'list' objects>, '__reversed__': <method '__reversed__' of 'list' objects>, 'extend': <method 'extend' of 'list' objects>, '__delitem__': <slot wrapper '__delitem__' of 'list' objects>, 'sort': <method 'sort' of 'list' objects>, 'insert': <method 'insert' of 'list' objects>, 'reverse': <method 'reverse' of 'list' objects>, '__getitem__': <method '__getitem__' of 'list' objects>, '__repr__': <slot wrapper '__repr__' of 'list' objects>, '__doc__': "list() -> new empty list\nlist(iterable) -> new list initialized from iterable's items", '__sizeof__': <method '__sizeof__' of 'list' objects>, 'pop': <method 'pop' of 'list' objects>, '__iadd__': <slot wrapper '__iadd__' of 'list' objects>, '__hash__': None, '__add__': <slot wrapper '__add__' of 'list' objects>, 'index': <method 'index' of 'list' objects>, '__le__': <slot wrapper '__le__' of 'list' objects>, '__ne__': <slot wrapper '__ne__' of 'list' objects>, '__len__': <slot wrapper '__len__' of 'list' objects>, 'append': <method 'append' of 'list' objects>, '__init__': <slot wrapper '__init__' of 'list' objects>, 'count': <method 'count' of 'list' objects>, '__eq__': <slot wrapper '__eq__' of 'list' objects>, '__getattribute__': <slot wrapper '__getattribute__' of 'list' objects>, '__lt__': <slot wrapper '__lt__' of 'list' objects>, '__new__': <built-in method __new__ of type object at 0x10091cd30>})
```

13. input([prompt])

用来从命令行获取输入，对于命令行软件非常实用的一个命令，实用起来也非常简单：

```
>>> s = input('--> ')
--> >? halou
>>> s

'halou'
```

14. isinstance(object, classinfo) issubclass(class,classinfo)

- isinstance用来判断一个对象是某一个类的实例。
- issubclass用于判断对象是不是某个类的子类。

值得一提的是，一个类被认为是自己的一个子类，详见例子中的`issubclass(A,A)`

```
class A:
    a = 1


class B(A):
    b = 2


a = A()
b = B()

>>> isinstance(a,A)
True
>>> isinstance(b,A)
True
>>> issubclass(B,A)
True
>>> issubclass(A,A)
True
```

15. memoryview
memoryview类似于C语言中的指针，他提供了一种直接访问内存的方式。memoryview可以用于支持`Buffer protocol`的对象，python内置对象中的
bytes和bytearray支持这种协议。memoryview一般用于对巨大对象的操作，他可以直接访问原对象的内存而不对原对象进行拷贝。

假设我们有一个很长的字符串'lellolello...', 我们需要把其中的'lello'全都替换成'hello',那么通过memoryview就会大大提高效率，详见示例代码：
```
import time

long_str = 'bello'*100000
start = time.time()
for i in range(0,len(long_str),5):
    long_str = long_str[:i]+'h'+long_str[i+1:]
print("time for str replace : {}".format(time.time()-start))

long_str = 'bello'*100000
start = time.time()
memoryview_of_str = memoryview(bytearray(long_str,'ascii'))
for i in range(0,len(memoryview_of_str),5):
    memoryview_of_str[i] = ord('h')
long_str = memoryview_of_str.tobytes().decode('utf-8')
print("time for memory view: {}".format(time.time()-start))
```
这段代码的执行结果如下：
```
time for str replace : 13.332601070404053
time for memory view: 0.016571044921875
```
大家可以看到，速度相差非常大，这也就是对大对象进行拷贝的代价。这个例子可能不是memoryview的典型使用场景（这里使用list也会很搞笑），但是这个例子证明了对字节直接进行操作的效率。


16. zip()
将两个或多个iterable结合成一个iterable，其中的每个元素是一个tuple,详见下面示例

```
>>>for i in zip(['a','b','c'],[1,2,3],[9,8,7]):
>>>    print(i)
('a', 1, 9)
('b', 2, 8)
('c', 3, 7)
```


17. __import__() 

一种高级的import方法，如果不了解这个方法就尽量不要使用，因为该方法并不是为日常开发设计的，可以考虑使用`importlib.import_module()`替代。