---
layout: post
title:  "宽边缘Softmax loss"
date:   2018-04-05 11:00:00 +0800
categories: Machine Learning
---

图像识别领域的技术已经比较成熟，在拥有大训练集的情况下图像识别的准确度已经可以基本满足商业应用的需求。虽然技术框架已经基本成熟，但是学术界还在继续对模型细节进行优化，其中损失函数式2017年的研究热点之一。下文是对一篇损失函数方面的优秀论文 [Large-Margin Softmax Loss for Convolutional Neural Networks][paper-link] 的简要总结。

## 原文摘要的摘要
Softmax+Cross entrophy一直是CNN中最常用的损失函数，因为他简单有效。不过softmax也存在一些自己的不足，他并不显示的强化特征学习过程（后边会有详细解释）。 L-softmax能显式的鼓励类内剧集和雷剑分离。L-softmax可以通过普通的梯度下降算法进行优化，还能抑制过拟合。

## Softmax Loss问题在哪里？
Softmax loss有很多有点，他计算简单，效果不错，帮助各种不同的模型取得了很好的效果。但是让我们想一下在一个二分类问题中，要保证分类正确，Softmax函数的要求是什么？

如果样本x属于第一类，那么只需要W<sub>1</sub>*x 大于 W<sub>2</sub>*x 即可，即使W1*x 和 W2*x 的值很接近，也可以满足需求。从感觉上来说，这个要求有点低了，因为我们希望不同类之间的区别大一些，这样可以增加鲁棒性。

## L-softmax 的原理？

L-softmax的思路也很简单，就是让我们在分类的时候不但能把两类分开，还要让不同类之间有一定距离，这个思想和SVM中的有些相似。

![l-softmax](/images/l-softmax-formula.png?style=center "l-softmax")

上式中第一行左侧代表传统softmax正类的值，右侧代表l-softmax中正类的值，第二行代表负类的值的值。其中的m是一个超参数而且必须是正整数。不难看出l-softmax通过在角度中增加了一个m使得不等式的要求更加严格。

具体的损失函数的定义可以在论文中找到，由于本文不讲技术细节就不贴过来了。

## L-softmax 效果

下图是论文中贴出的算法效果图，其中最左侧（m=1）即为传统的softmax loss。着重观察图中同类的距离和类间距离我们可以发现l-softmax的结果中类之间的划分更加清晰。更加具体的实验数据可以在论文中找到。

![l-softmax-performance](/images/l-softmax-performance.png?style=center "l-softmax-performance")

## 思考

本文提供了一种优化softmax的思路，在softmax中加入了边界的概念，提升了算法的鲁棒性。但是算法本身的实现复杂度比普通softmax要高很多，在大规模数据训练时对资源的要求也会有相应提高。所以本算法在实际应用中仍然需要评估其投入产出比。





[paper-link]:http://proceedings.mlr.press/v48/liud16.pdf