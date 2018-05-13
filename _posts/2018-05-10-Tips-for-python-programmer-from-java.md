---
layout: post
title:  "Java开发者学习Python的注意事项"
date:   2018-05-10 11:00:00 +0800
categories: Python
---

Java作为最流行的语言在软件开发行业有很大的影响，在Java中建立起的软件开发方式也深深地影响了其他语言的开发。但是Python作为一门设计理念与Java有很大不同的语言，开发时和Java有诸多不同。由于设计理念不同, 如果使用Java的方式去写Python代码会极大地增加复杂度并降低开发效率。下面列出几个Python与Java的显著区别，希望可以帮助Java开发者更快的理解Python编程。

1. Java中的静态方法不等价于Python中的类方法，而是更接近于Python中的模块方法。如果有静态方法的需求而且不需要调用类变量，那么你很可能需要一个模块方法而不是使用`@staticmethod`修饰符。
Foo.Foo.someMethod when it should just be Foo.someFunction

2. 在Python 3中`//`代表整数除法，Python 3引入这个运算符是因为在Python 2中`/`即为整数除法，Python 3中将`/`运算符的行为进行了修改，所以又引入了`//`来保留原来的整数除法功能。但需要注意的是Python3中的`//`与Python2中的`/`行为不完全相同。下面是在Python2和3中的计算结果。
    ```
    Python 2
    5/3=1
    5.0/3=1.6666666666666667

    Python 3
    5/3=1.6666666666666667
    5.0/3=1.6666666666666667
    5//3=1
    5.0//3=1.0
    ```

3. Java初学者遇到的一个大坑就是用`==`去比较两个字符串，Java会去比较字符串的地址而不是内容。但在Python中没有这个问题，`==`会针对重写  可以用来比较string

4.  在Python中不是用||, &&和！这些常见的逻辑运算符，而是使用`and`,`not`和`or`，这更加易读。

5. Python中没有switch关键字。如果有类似的需求，最简单的方法是讲switch写成一个长if-else，但这种方法非常冗长，并不漂亮。最好的办法是将switch翻译成一个dict。一个简单的例子如下，下面的代码将会输出20.
    ```
    calculate = {
        '+':lambda x,y: x+y,
        '-':lambda x,y:x-y,
    }

    action='+'
    x=10
    y=10
    calculate[action](x,y)
    ```

6. 一般不要使用xml:
在Java中我们大量使用xml文件，这帮助Java开发者解决了很多问题。但是在Python中一般不提倡使用Xml文件，因为相对Python来说xml文件比较复杂，使用它可能并不会比直接写Python代码来的高效。不过这不意味着我们不能使用xml（目前的Python标准库中也提供了XML处理库`ElementTree `），只是先使用时应该更慎重，。

7. 不需要getter和setter:
在Java中如果将类变量设置为public，你就失去了再后面开发中取回变量控制权的机会（除非重构代码），这也是为什么在Java中我们常常会为变量生成getter和setter。但是在Python中我们只需要直接调用类的变量，如果日后我们需要对变量进行包装（如增加数据验证功能），可以使用Python的`@property`特性来实现。

8. 在Python中，并不是所有东西都要放到Class中。直接将方法写到模块中没有任何问题，只要它能满足你的需求。此外，在Python中并不要求一个文件中只包含一个类，这也是和Java的一个明显的不同点。

9. Python中不需要用接口来重写类方法。在Java中我们会写一个Animal接口然后实现一个Cat和一个Dog。在Python中我们不需要Animal接口，因为Python并不会检查参数类型（参考Duck Typing）。

10. Python中没有严格的私有变量，如果想让某个变类量不背外部访问，可以再变量名前加上`__variable_name`,但是这只是一个约定告诉其他人这个变量不应该在类外访问，但你并不能限制其他人访问该方法。这种设计背后隐藏的是python的设计哲学"We are all consenting adults here."


