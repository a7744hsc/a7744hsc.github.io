---
layout: post
title:  "Go over Cpp"
categories: Programing Language
---
Go through C++
=============

20170125, Basics
-------
Build-in type: 
* int 

Return value of main function: 0 indicates success.

cin, cout, clog, cerr


char  unsigned char

use int and double (but they veried from machine to machine)

Literal, 常量

Four different ways to initialize a new variables:
* int units_sold = 0; 
* int units_sold = {0}; 
* int units_sold{0};
* int units_sold(0);

When initialize with curly brace, compiler will not let us list initialize variables of built-in type if the initializer might lead to the loss of information.(new in C++11)

Uninitialized objects of built-in type defined inside a function body have a undefined value. Objects of class type that we do not explicitly inititalize have a value that is defined by class. 

Initializing every built-in type is always recommanded unless you are sure it is safe to omit it.

Use `extern` keyword to declare but not define a variable (We can provide an initializer on a variable defined as extern, but doing so overrides the extern. An extern that has an initializer is a definition).

It is an error to provide an initializer on an extern inside a function.

A reference is declared by writing a declarator of the form `&d`, it must be initialized and can not be rebind it.

A pointer holds the address of another object, it is declared with a declarator of form `*d`, pointer is an object in its own right(can be assigned and copied).

We should use nullptr to initialize a null pointer and should avoid using NULL.

判断变量类型从右向左阅读，离变量最近的修饰符决定变量类型。

const变量默认是只在文件内有效， 如果要跨文件使用同一个const变量，必须在定义和声明时都是用extern修饰符

Lvalue,Rvalue????   low-level const(only related to conpound type's base type), high-level const(means arbitrary variable is a constant)

constexpr 常量表达式

类型别名（Type alias） typedef name alias; | using alias = name; 

decltype 可以用来推断表达式返回值类型， 如果的decltype的参数加上括号 `decltype((v))`结果是引用

类定义后面加上分号，

C++11, 类内初始值，range for for(type element:iterator),

关于头文件：
* 头文件通常包含只能被定义一次的元素，如class，const，constexpr。
* 头文件也会include一些需要用到的其他头文件
* 不要再头文件中使用using声明，这样可能造成命名冲突




C++流：输入输出流，文件流，string流 (iostream,fstream,stringstream)

顺序容器Sequential Container：Vector, deque, list, forward_list, array, string.
容器适配器，接受一个已有容器使其看起来像另外一个容器。 stack,queue,priority_queue.


Lambda 表达式：
[ capture-list ] ( params ) mutable(optional) constexpr(optional)(c++17) exception attribute -> ret { body }
[ capture-list ] { body }
auto glambda = [](auto a, auto&& b) { return a < b; };

关联容器：map set   multimap multiset     无序关联容器  unordered_set ...
关联容器的主要方法： insert 下标操作 find count

动态内存与智能指针：
shared_ptr允许多个指针指向同一对象，unique_ptr独占对象 weak_ptr指向shared_ptr管理的对象。  他们在memory头文件中。

make_shared<type>() 可以生成一个shared_ptr

拷贝操作，运算符重载，模板编程，tuple，bitset，随机数，多继承，虚继承
