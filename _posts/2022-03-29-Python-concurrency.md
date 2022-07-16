---
layout: post
title:  "Python并发编程"
date:   2022-03-29 01:00:00 +0800
categories: Python
---

# Python 并发编程

## 引言
Python的并发一直是被社区吐槽的一个点，由于很多历史原因标准Python解析器（CPython）的多线程（threading）并不能完全利用现代多核CPU的全部能力。

社区在抱怨Python性能的同时也开发出了很多解决方案，这些解决方案从不同的角度提升了python的性能，这些三方库是Python成功的巨大助力。Twisted，GEvent，Tornado和Numpy就是三方方案中比较著名的例子。

大量三方库的出现，尤其是如twisted置类的三方并发方案的大量出现也为学习Python并发编程增加了难度。尤其是最近几年Python第一方并发库的完善和asyncio库的出现使得初学者更无从下手,(StackOverflow上关于python并发的问题)[https://stackoverflow.com/questions/61351844/difference-between-multiprocessing-asyncio-and-concurrency-futures-in-python]

## 需求
一个web框架性能评估网站https://www.techempower.com/benchmarks

## 方案选择
PyPy,Cython   Python的发行版
Django，Flask，FaskAPI，Tornado，GEvent，Cyclone，Twisted，Pyramid
threading&queue, multiprocessing,asyncio, concurrent.futures

Concurrency and Parallelism
multiprocessing 
concurrent.futures
https://wiki.python.org/moin/ParallelProcessing
https://sebastianraschka.com/Articles/2014_multiprocessing.html
https://medium.com/idealo-tech-blog/parallelisation-in-python-an-alternative-approach-b2749b49a1e
https://realpython.com/python-concurrency/


python threading 包 提供的锁：
Lock: 最基础的锁
RLock: 可重入锁, 可以在同一个线程中多次获取锁
Condition Object: 条件变量，可以传入一个锁，获取锁后，可以调用wait释放锁，然后等待条件成立（被notify时），再次获取锁，然后继续执行。
Semaphore: 信号量，可以控制同时访问的线程数量。





