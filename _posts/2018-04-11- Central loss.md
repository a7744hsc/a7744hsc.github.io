---
layout: post
title:  "CNN损失函数优化-Central loss"
date:   2018-04-11 11:00:00 +0800
categories: Machine Learning
---

上一周我们介绍了[款边缘Softmax loss][wide-margin-softmax-loss], 这周我们来介绍另一种损失函数优化的方式, Central loss，论文链接在[这里][http://ydwen.github.io/papers/WenECCV16.pdf]。

## 文章摘要：

在图像识别中，我们通过卷积提取特征，底层的全连接层是一个线性分类器，用来区分数据类别。实际应用中，我们不但期望去区分类别，而且希望类间距够大，这样能增加鲁棒性。该论文提出了一种叫做中心损失的监督学习损失函数，该函数能够强化深度特征的区分度。该算法将特征和特征中心的距离加入惩罚项，从而学习每个深度特征的中心。该算法可以通过反向传播训练，并能够在CNN网络中进行优化。通过联合使用Softmax loss和Center loss，我们可以训练出具有类内剧集紧密，类间距离明显的特征。

## 算法思路解释
和款边缘softmax-loss一样，Central loss 也是希望增大类间距离来提升模型鲁棒性。在宽边缘loss中算法通过改变损失曲线来强制让特征和正例样本方向一致，而中心loss是通过下面的损失函数让正例和特征的2阶范数更接近。下式左侧代表损失，右侧的C<sub>yi</sub>为第类的中心，该中心通过对该类样本求平均值得到，在论文中有具体计算方法。

![center_loss_formula](/images/center_loss_formula.png?style=center "center_loss_formula")

## 特征中心计算思路

由于深度神经网络训练数据巨大，目前多数网络都使用mini-batch BP这种算法来训练网络，这种情况下损失函数的选择收到很多限制，论文原文也认为这是中央损失算法直到现在才被提出的原因。针对这个问题，论文提出了两个非常简单地办法来解决，这两个办法在解决其他深度学习问题时也很常见。

1. 不用全数据去计算特征中心C，而是和BP一样使用mini batch来更新特征中心C。
2. 为了解决mini batch更新不稳定的问题，为更新动作加入了超参数a，防止中心变化过大，这个和momentum算法思想类似。

## 损失函数

本文提出了中心损失函数，但是在实际训练时不能只使用中心损失函数，因为该函数倾向于让特征点接近中心点，论文中使用的损失函数结合了softmax loss 和 central loss，取得了不错的效果。结合方法是简单地把两种损失相加，公式如下：

![combine_CL_SL](/images/combine_CL_SL.png?style=center "combine two loss together")


## 样本分布可视化

本文通过将全连接层输出维度变为2使样本的特征分布可以直接可视化，这种可视化思路非常有意思，并且成功的引起了损失函数调优的狂潮（该方法应该为本文提出，但是没有核实过）。


## 算法效果

论文原文有关于算法效果的详细对比数据，这里我们贴出一张图来简单解释一下：

![performance_of_central_loss](/images/performance_of_central_loss.png?style=center "performance_of_central_loss.png")

从图中可以看出，lambda（代表中心损失的重要性）越大，特征聚集的越紧密，相应的系统鲁棒性就约好。
 




 [wide-margin-softmax-loss]:https://a7744hsc.github.io/machine/learning/2018/04/05/Large-margin-softmax.html