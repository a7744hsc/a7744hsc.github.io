---
layout: post
title:  "Python(python 3)的内置方法-上"
date:   2018-05-24 11:00:00 +0800
categories: Python
---

Python 3.6.5中包含68个内置方法,[官方文档](https://docs.python.org/3/library/functions.html)对这些方法都有详细的介绍。在这68个方法中包含`abs`,`dict`,`len`等大家常用的方法，也包含了`ascii`,`memoryview`和`repr`等不那么常见的方法。 本文会对以下相对少用的方法进行简要介绍(由于方法数量比较多，部分方法将在"Python(python 3)的内置方法-下"中介绍)：

1. all(iterable), any(iterable)
    用于判断一个iterable是否全都为True（all），或至少存在一个True（any）：
    ```
        >>> all([1,2,3,'ad',True])
        True
        >>> all([0,2,3,4,5])
        False
        >>>any([0,False,1])
        True
        >>>any([0,False,''])
        False
    ```
    - all方法可以帮我们轻松判断一组字符串中是否有None或者空值，
    - any方法可以用来判断数组中是否存在期望的值

    ```
    >>>str_list = ['foo','bar','not empty']
    >>>all(str_list)
    True

    >>>num_list = [1,2,3,4,5]
    >>>any([i==5 for i in num_list])
    True
    ```


2. ascii(object), repr(object)

    `ascii`和`repr`方法类似，都是将对象以字符串的形式返回，以方便打印。不过在python3中，`ascii`方法会将非ascii字符转换以编码形式输出：

    ```
    >>>repr('非ascii字符')
    "'非ascii字符'"

    >>>ascii("非ascii字符")
    "'\\u975eascii\\u5b57\\u7b26'" # 这里会将中文字符以utf-8的编码形式展示出来。
    ```

3. bin(x)
    将整数转换成二进制字符串
    ```
        >>>bin(255)
        '0b11111111'
        >>>bin(-10)
        '-0b1010'
    ```

4. enumerate(iterable, start=0)

    当我们在对iterable进行迭代操作时需要用到索引时可以通过`enumerate `来实现：

    ```
    >>>l=[5,4,3,2,1]
    >>>for index,value in enumerate(l):
    >>>    print("for index {}, the value is {}".format(index,value))

    for index 0, the value is 5
    for index 1, the value is 4
    for index 2, the value is 3
    for index 3, the value is 2
    for index 4, the value is 1
    ```

5. callable(object)

    判断1个对象是否可以被调用：
    ```
        >>>l = [1,2,3]
        >>>callable(l)
        False
        >>>callable(l.append)
        True
    ```

6. chr(integer), ord(character)

    `chr`方法可以将 ascii 或者unicode转换成对应字符，而ord正好相反，将字符转换成对应的unicode编码

    ```
    >>>ord("好")
    22909
    >>>ord('a')
    97

    >>>chr(22909)
    '好'
    >>>chr(97)
    'a'
    ```

7. eval(expression, globals=None, locals=None），exec(object[, globals[, locals]])¶

    `eval()`可以执行字符串中的python语句(只能是单一表达式，不能使代码段)，并返回代码段的返回值。可以通过globals和locals两个参数来控制实行代码的上下文，如果两个参数都不提供，则使用调用处的上下文来执行。

    `exec()`用来执行代码片段，但是该方法总返回None

    ```
    >>>x=1,y=2
    >>>a = eval("x+y")
    >>>print(a)
    3

    >>>b = exec("x+y")
    >>>print(b)
    None
    ```

8. compile(source, filename, mode, flags=0, dont_inherit=False, optimize=-1)

    将字符串编译成可执行代码，mode包括3中模式，'exec'对应多行代码，'eval'对应单行表达式，‘single’对应交互时表达式，执行后会将返回值打印出来。
    示例如下
    ```
    >>>str = "for i in range(0,3): print(i)" 
    >>>executable_code = compile(str,'','exec')
    >>>exec(executable_code)
    0
    1
    2

    >>>str2 = '5'
    >>>executable_eval = compile(str,'','eval')
    >>>executable_single = compile(str,'','single')

    >>>exec(executable_eval)
    #执行过后无任何输出

    >>>exec(executable_single)
    5
    ```
			

9. dir([object])

    无参调用时返回当前本地作用域内的变量(包括系统内置变量和import进来的对象)，有参数时尝试返回对象的属性(包括方法和子类)。`dir()`方法会尝试调用`__dir__()`，如果`__dir__()`不存在则会尝试读取`__dict__`变量。

    ```
    >>>dir() #在一个新的Ipython环境下调用，不同的环境结果可能会有不同。
    ['In',
    'Out',
    '_',
    '__',
    '___',
    '__builtin__',
    '__builtins__',
    '__doc__',
    '__loader__',
    '__name__',
    '__package__',
    '__spec__',
    '_dh',
    '_i',
    '_i1',
    '_ih',
    '_ii',
    '_iii',
    '_oh',
    '_sh',
    'exit',
    'get_ipython',
    'quit']

    >>>dir(list)
    ['__add__',
    '__class__',
    '__contains__',
    '__delattr__',
    '__delitem__',
    '__dir__',
    '__doc__',
    '__eq__',
    '__format__',
    '__ge__',
    '__getattribute__',
    '__getitem__',
    '__gt__',
    '__hash__',
    '__iadd__',
    '__imul__',
    '__init__',
    '__iter__',
    '__le__',
    '__len__',
    '__lt__',
    '__mul__',
    '__ne__',
    '__new__',
    '__reduce__',
    '__reduce_ex__',
    '__repr__',
    '__reversed__',
    '__rmul__',
    '__setattr__',
    '__setitem__',
    '__sizeof__',
    '__str__',
    '__subclasshook__',
    'append',
    'clear',
    'copy',
    'count',
    'extend',
    'index',
    'insert',
    'pop',
    'remove',
    'reverse',
    'sort']
    ```



10. filter(function,iterable),map(function,iterable)
    
    filter方法可以很方便的根据一定跳进过滤iterable对象，`filter(function, iterable)` 等价于生成器表达式：`(item for item in iterable if function(item)) `。下面的例子可以从数组中取出所有奇数：

    ```
    l=[1,2,3,4,5,6]

    >>> def is_odd(i):
        return bool(i%2)

    >>>list(filter(is_odd,l))
    [1,3,5]
    ```


<!-- 11. frozenset(iterable)

12. globals()，locals()

13. input([prompt])

从命令行获取输入

14. isinstance(object, classinfo) issubclass(class,classinfo)

- isinstance用来判断一个对象是某一个类的实例。
- issubclass用于判断对象是不是某个类的子类。

15. memoryview


16. zip()

17. vars()

18. __import__() -->