---
layout: post
title:  "Python -m参数解析"
date:   2018-05-03 11:00:00 +0800
categories: Python
---

注: 本文中所有实验都在python3.6下进行

## 为什么引入这个选项？
python的执行文件中包含一个`-m`的选项，这种用法最初在python2.4中引入，随后经过[PEP338](https://www.python.org/dev/peps/pep-0338/)的修改成为现在这种形式。从PEP中我们可以知道该参数出现的目的是帮助我们更方便的执行python库中的脚本，而各种库也可以利用这种方式来提供一些实用的功能。

## 该参数具体功能是什么？
该选项在python的帮助文档的说明如下:
> -m mod : run library module as a script (terminates option list)

即将python库中的模块以脚本的模式运行（并截断选项列表）。说明十分简洁，那这句话到底是什么意思呢？直接执行`python aaa/bbb.py` 和`python -m aaa.bbb`又有什么区别呢？下面我们通过一个实验来回答上面的问题。

我们来执行一段简单的实验代码([Github链接](https://github.com/a7744hsc/python-features/tree/master/run-lib-as-script))，代码如下：

1. 总体结构
    ```
    ├── outter.py
    └── package1
        ├── __init__.py
        └── main.py
    ```

2. outter.py
    ```
    def load_outter_method():
	    print("outter method excuted")
    ```

3. \_\_init__.py
    ```
    print("package1 loaded")
    ```

4. inner.py:
    ```
    import sys
    # from outter import load_outter_method

    print("module inner loaded")

    if __name__ == "__main__":
        print("module __main__ method excuted")
        print("package name:",__package__)
        print("package name:",__name__)
        print("path",sys.path)

        # load_outter_method()
    ```

我们在`run-lib-as-script`分别通过两种方式执行`inner.py`

执行 `python package1/inner.py`的输出:
```
module inner loaded
module __main__ method excuted
package name: None
package name: __main__
path: ['/Users/twer/PROJECTS/python/python-features/run-lib-as-script/package1', '/Users/twer/anaconda/envs/eminfo_backend/lib/python36.zip', '/Users/twer/anaconda/envs/eminfo_backend/lib/python3.6', '/Users/twer/anaconda/envs/eminfo_backend/lib/python3.6/lib-dynload', '/Users/twer/anaconda/envs/eminfo_backend/lib/python3.6/site-packages']
```

执行 `python -m package1.inner`的输出:
```
package1 loaded
module inner loaded
module __main__ method excuted
package name: package1
package name: __main__
path: ['', '/Users/twer/anaconda/envs/eminfo_backend/lib/python36.zip', '/Users/twer/anaconda/envs/eminfo_backend/lib/python3.6', '/Users/twer/anaconda/envs/eminfo_backend/lib/python3.6/lib-dynload', '/Users/twer/anaconda/envs/eminfo_backend/lib/python3.6/site-packages']
```

从上面两个输出的对比我们可以发现两种执行方式有以下几点不同:
1. 通过`python -m`执行一个包内脚本会首先将执行package1的`__init__.py`文件，并且`__package__`变量被赋上相应的值；而 `python xxx.py`方式不会执行`__init__.py`并且`__package__`变量为None
2. 两种执行方法的`sys.path`不同（注意每个path输出中的第一条），Python中的`sys.path`是Python用来搜索包和模块的路径。通过`python -m`执行一个脚本时会将当前路径加入到系统路径中,而使用`python xxx.py`执行脚本则会将脚本所在文件夹加入到系统路径中（如果取消inner.py中的注释会报找不到模块的错误）。

所以如果是运行自己写的简单脚本，两种运行方式可能没有区别，但是如果运行三方包中的脚本使用`python -m`会把整个包加入到sys.path中，可以减少引用问题。


## python -m 的实现原理

python中有一个叫做[runpy](https://docs.python.org/3/library/runpy.html)的模块，这就是`python -m`的具体实现（执行`python -m modulename`等同于运行runpy.run_module(modulename)）。如果想要进一步了解`python -m`可以阅读该模块源代码。

## Python -m 的的实用功能：

- `python -m venv` 创建虚拟环境（venv是python3自带的环境管理工具，和virtualenv用法类似）
- `python -m http.server` 启动一个http服务器，可以用于文件共享
- `python -m idlelib` 启动IDLE，一个python自带可开发环境

更多实用模块脚本可以再[这里](https://github.com/cassiobotaro/awesome-python-modules-as-script)找到