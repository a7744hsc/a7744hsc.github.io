---
layout: post
title:  "Python的引用机制"
date:   2018-07-8 01:00:00 +0800
categories: Python
---

Python作为解释型语言和Java等编译型语言的引用机制有很大区别，本文将浅析Python的引用机制，方便我们避免引用异常。想写这个是因为我在开发过程中遇到的一个问题，下面的文章也已解决问题为线索展开。

## 错误
在工作中写了一段与下面示例类似的代码并出现了引用异常。
错误信息可以通过执行下面示例代码中的`t2.py`复现，报错信息为`ImportError: cannot import name 'create_b'`。

t1.py
```
from t2 import create_b


class A:
    def __init__(self, v1):
        self.v1 = v1
        self.v2 = create_b(v1)


class B:
    def __init__(self, v1):
        self.v1 = v1


if __name__ == '__main__':
    a = A('test')
```

t2.py
```
from t1 import B


def create_b(v1):
    v_temp = v1+'modified'
    return B(v_temp)
```

## 错误分析

通过阅读代码和搜索错误信息（主要看了[这篇文章](https://stackoverflow.com/questions/11698530/two-python-modules-require-each-others-contents-can-that-work)）, 我发现引发问题的原因是两个模块相互引用。 当运行`t1.py`时，他会去引入`t2.py`; 在`t2.py`运行时，会去引入`t1.py`中的`B()`; 但是此时`t1.py`中的`class B`还没有载入到内存中，无法找到，导致了引用异常。

修复错误的方法有两种：

1. 使用`import xxx`代替`from xxx import xxx`
2. 将引用放到模块末尾

下面是使用第二种方法的示例代码。

t1.py
```
class A:
    def __init__(self, v1):
        self.v1 = v1
        self.v2 = create_b(v1)


class B:
    def __init__(self, v1):
        self.v1 = v1


from t2 import create_b

if __name__ == '__main__':
    a = A('test')
```

t2.py
```
def create_b(v1):
    v_temp = v1 + 'modified'
    return B(v_temp)


from t1 import B
```

## Python 引用机制

Python作为一个动态语言会在执行代码时处理引用，引用主要分为下面两种形式:
1. `import x`， 这里称为第1类引用
2. `from x import xxx`， 这里成为第二类引用

这两种形式的引用略有不同，但总体引用流程相同，主要处理流程分为以下几步：
1. 当引用一个模块时，python会先从`sys.modules`中查找当前上下文是否已经存在。
2. 对于第1类引用，如果目标模块已经存在（不关模块是否已经完全载入），则只需在当前上下文建立一个对该模块`x`的索引即可；对于第2类引用则需要建立一个对`x.xxx`的引用。
3. 当模块不存在时，会找到该模块并执行，将执行后获得的对象加入到`sys.modules`中。当导入时遇到其他`import`子句的时候，则会递归的执行上述步骤。当模块导入成功后，会重新进行第二步。

另外，需要注意的是，python在引入modules的时候会执行模块级别的代码（模块方法，模块变量等），但是对于模块的Functions，在引入代码时并不会被真正执行，而只是记录方法的声明并建立连接，真正的方法代码是在方法被调用时才会执行。

希望读完上面的介绍，我们可以更好的组织代码间的引用关系。除了循环引用之外，Python关于引用的另外一个难点是`绝对引用`和`相对引用`，感兴趣的读者也可以自己去查询一下，或者读我后续发布的关于这两种引用的文章。