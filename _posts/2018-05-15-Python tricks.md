---
layout: post
title:  "Python(python 3)小技巧，你知道几个？"
date:   2018-05-15 11:00:00 +0800
categories: Python
---

1. `python -m http.server`

    在8000端口启动一个web服务器，可以用于机器之间的文件传输(类似一个FTP服务器)
    
    更多实用的Python库脚本可以在[此处](https://a7744hsc.github.io/python/2018/05/03/Run-python-script.html)找到。

2. `import this`
打印[PEP 20](https://www.python.org/dev/peps/pep-0020/) 的内容. 这是由Tim Peters提出的Python设计原则。

3. 使用Python内置的zip命令可以将两个list组合起来，示例如下。
    ```
    b = [[1, 2, 3, 4], [6, 5, 4, 3]]
    zip(*b)
    [(1, 6), (2, 5), (3, 4), (4, 3)]
    ```

4. 使用Python中切片操作来将数组倒叙（也可以用于字符串）
    ```
    l=[1,2,3,4,5]
    l[::-1]
    [5, 4, 3, 2, 1]

    s = "Hellow World"
    s[::-1]
    'dlroW wolleH'
    ```
5. 不需要创建中间变量即可交换两个变量的值
    ```
    a = 1
    b = 3
    b,a = a,b
    print(a)
    3
    print(b)
    1
    ```
6. 合并两个dict对象
    ```
    a={"A":1}
    b={"B":2}
    {**a,**b}
    {'A': 1, 'B': 2}
    ```
7. 在解释器模式下，可以使用`_`来获取最后一次的输出结果
    
    ```
    >>>100*100
    10000
    >>>_
    10000
    ```

8. 快速生成一个日历
    ```
    import calendar
    cal = calendar.month(2018,5)
    print(cal)
    ```
9. 快速得到无穷大和无穷小：
    ```
    float('inf') #无穷大
    float('-inf') #无穷小
    ```

10. Python可以很方便的支持复杂的比较，下面是一些示例：
    ```
    x = 2
    3 > x == 1
    -> False
    1 < x < 3
    -> True
    10 < 10*x < 30 
    -> True
    10 < x**5 < 30 
    -> False
    100 < x*100 >= x**6 + 34 > x <= 2*x <5
    -> True
    ```
10. Enumerate，可以方便的应对List的迭代并需要index的场景:
    ```
    l=[1,2,3]
    for index,value in enumerate(l):
        print(index,value)
    Output:
        0 1
        1 2
        2 3
    ```
11. 使用zip将dict的key和value对调。
    ```
    d = {'a': 1, 'b': 2, 'c': 3, 'd': 4}
    dict(zip(d.values(),d.keys()))
    Output:
        {1: 'a', 2: 'b', 3: 'c', 4: 'd'}
    ```

12. Python的[collections模块](https://docs.python.org/3/library/collections.html)，里面包含很多有用的数据类型。由于内容比较多，后续会单独写文章介绍。

<!-- 隐藏信息！！！！！！-->
<!-- Python变量查找顺序：
LEGB： local->enclosing->global->built-in -->