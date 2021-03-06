---
layout: post
title:  "Clean Code读书笔记"
date:   2018-06-22 01:00:00 +0800
categories: Other
---

## 前言
很久以前就听说过Clean code这本书的大名，但是一直没有时间拜读。而我各人也一直认为在没有任何经验的情况下读这种这种书很难领会其真正的意图。最近读了这本书，还是有很多收获的。

## 一些要点
下面是我从书中总结出的一些比较重要，需要在开发时考虑的点：

1. 提高代码质量真的很重要，一味追求速度很可能在项目后期带来灾难，尤其是长期项目。个人认为这个道理只有通过经验得来才足够深刻。

2. 什么是好代码？这很难定义，我个人认为需要符合下面三点：

    - 可以按照需求工作
    - 易读（这其中包括减少重复，精简实体等等）
    - 易扩展

3. 命名是小事，但是真的很重要。
    - 统一项目语言，杜绝同时使用fetch，retriece和get等同义词。

4. 简单易读才重要，复杂晦涩的代码并不会带来好处。 命名时也不要使用俗语或者典故。

5. 函数：
    - 函数应该短小，可以把功能代码都抽象出去。？
    - 每个函数只做一件事，尽量不包含多层嵌套结构。
    - 函数应该有层次之分，不同层次的函数不应该混淆在一起。
    - 函数参数越少越好，尽量不要超过三个
    - 无副作用
    - 异常处理和逻辑代码分离


6. 注释是对错误的一种补救，可以写有意义的注释，但是一定要在修改代码时更新注释。

7. 区分对象和数据结构，不要在数据结构中引入处理方法也不要再对象中暴露其数据。

8. 错误处理不应该扰乱代码逻辑，也就是说错误处理不应该和逻辑代码混杂在一起。

9. 不要反回null，也不要讲null作为参数使用，这会使代码充满null判断，也很容易引发问题。

10. 可以通过编写测试来了解第三方代码。

11. 每个测试包含一个概念

12. 类的职责应该单一


## 感受

可能上面一部分内容还是比较干，不太容易领会，而我也觉得如果没有写烂代码的经验，这些教条的东西很难让人信服。不过书中一整章的关于参数解析器的案例分析却让我学到很多。认真读读这一章，了解一个小段代码如何随着时间的推移变得越来越难以维护，让我更信服书中所说的一些例子。

而下面这一小段话是我读完整数的感受，是无论有没有经验，下面这段话都应该很容易被认同，这也是我们在开发时应该牢记的指导思想：

    写代码不但要能工作，还要让人能看得懂，因为对一个项目来说，99%的可能性将来会有人需要重读，甚至修改你的代码；即使这只是你个人的项目的项目，那么将来你也很可能需要重读自己的代码，修改自己的代码。所以当你开发的时候，不但要让程序能运行，还要让别人能看得懂（而不是自己看得懂）。

    一个复杂的项目往往有很多代码，如果代码全都揉在一起，我每次修改业务逻辑的时候都要遍历每一个细节，这会使工作变得十分复杂。
    而开发时引入层级的概念，将代码抽象成几层，这样不同目的的人就可以只去看对应层级的代码。如果我只是要了解业务逻辑，那么只需阅读较高层次的代码，而无需关注其下的技术细节。所以将代码分层是一个很有必要的事情。
