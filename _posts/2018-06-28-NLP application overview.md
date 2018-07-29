---
layout: post
title:  "不谈技术，NLP能做些什么"
date:   2018-06-28 01:00:00 +0800
categories: NLP
---

深度神经网络的爆发使得机器学习受到了广泛的关注, 这次博客大赛也有多篇这方面的文章, 而NLP（自然语言处理）作为机器学习领域的明星也受到了越来越多的人的关注。ThoughtWorks也对NLP技术的商业化落地非常关注, 对话机器人和针对特定领域的机器翻译等都是公司在这方面的初步尝试。

由于NLP的重要性和其最近几年的飞速发展, 它成为了人们经常会讨论的一个流行话题。对于专业人士来说, 通常会聊到词向量, LSTM, attention等技术。但是作为一个非NLP专家, 我们要聊些什么呢？或者说我们应该学些什么呢？ 本文就从应用角度来总结一下自然语言处理能做哪些事, 以及在我眼中NLP有潜力去做哪些事，帮助大家建立对NLP技术初步的理解。因为这是一个很大的话题, 所以下文难免会有疏漏之处, 欢迎大家回帖讨论。


### NLP能做那些事？

#### NLP的集大成者 - 语音助手

人类研究语音助手有几十年的历史, 这种形象也常常出现在各种科幻电影中, 不过直到2011年, 苹果发布Siri, 这种神秘的工具才获得了大众的关注。在Siri之后, 涌现出了以Amazon Alexa, Google Assistant, Microsoft Cortana为代表的一大票语音助手。早期的语音助手功能十分有限, 也很少有人使用。经过了这些年的发展, 现在的语音助手已经有了很大的进步, 对于天气查询, 信息检索, 添加日程, 播放音乐等简单任务已经能很好地处理。此外, 部分语音助手还支持声纹识别, 提升了安全性。如果你最近没有使用过上述的语音助手, 我强烈建议你去试用一下！

虽然语音助手是NLP的应用之一,  但其实她是一个复杂的综合性系统, 基本上使用了下文中提到的所有NLP技术以及很多其他非NLP技术。创建一个完整的语音助手需要大量的资源, 是一个门槛很高的领域。


#### 基于文本分类的应用
    
文本分类就是将非结构化文本数据划分到事先定义好的标签类中, 这是NLP技术的一大分支, 很多其他技术都依赖于它。由于分类任务不同, 标签的定义也不同, 比如在综合用户评论分析中, 标签可以定义为 "负面", "中性", "正面"。而在酒店评论分析中就可以把标签定义为"服务好", "环境好", "环境差"等。由于标签体系可以灵活调整, 文本分类被广泛应用到众多领域中, 下面列出一些典型的应用：

1. 垃圾邮件的检验：垃圾邮件检测的方法有很多, 其中一类就是利用文本分类技术来过滤垃圾邮件。

2. 新闻自动分组：对于分类新闻网站, 将新闻归类展示是一项消耗巨大的任务, 这里可以通过自动文本分类技术来自动化这一操作, 提升分类效率和用户阅读体验。

3. 用户情感分析（评论倾向性分析）：通过对用户评论进行分类（高兴or失望）处理, 可以得到用户对商家的态度度, 该方法已经在许多点评类应用中得到使用。

4. 文档自动标签, 搜索引擎优化(SEO)：通过文档自动分类得到新闻或web页面的标签, 将这些标签加入到网站的Head中能够起到优化搜索引擎排名的作用。



#### 基于命名实体识别（NER）的应用

命名实体识别的目标是定位文本中出现的预定义分类, 包括人名, 组织名称, 地名, 日期和时间, 数量等等。下面以一个例子来具体说明：
    
原文：
"Jim bought 300 shares of Acme Corp. in 2006."
标注后(括号内为实体类型)：
```[Jim](Person)bought 300 shares of [Acme Corp.](Organization)in [2006](Time).```


NER也有应用场景，下面是几个例子：
1. 新闻标注：和文本分类不同, 这里可以使用NER技术将与文章相关的人物, 地点都以标签的形式标注出来, 方便用户对某个人物或地点进行索引。

2. 搜索引擎：可以通过使用命名实体识别来抽取web页面中的实体, 后续可以使用这些信息来提高搜索效率和准确度。

3. 从商品描述中自动提取商品类别, 品牌等信息, 提高货物上架效率, 在咸鱼等应用上已经实现了类似功能。

4. 工具易用性提升, 例如从短信息或邮寄中提取时间和地点等实体, 从而实现点击时间直接创建日历, 点击地址直接跳转到地图App等便捷操作。


#### 其他

除了上面说到的几种分类之外, NLP还能做很多厉害的事情, 

1. 机器翻译：机器翻译是语音助手外另一个为大家熟知的NLP应用, 也是商业化最早的NLP应用。金山快译作为当年机器翻译市场的佼佼者是我最早接触到的几款软件之一。机器翻译刚出现时准确性较低, 不过随着近年来深度神经网络在机器翻译领域的成功应用, 目前的机器翻译已经有了很高的可用性。Google translate已经率先在生产环境部署了基于深度神经网络的翻译工具，是这方面的杰出代表。

2. 拼写检查（拼写纠错）：包括单词拼写检查, 句子正确性检查。拼写检查在搜索引擎上得到广泛应用, 当你在百度搜索"自然寓言处理"的时候, 百度会自动显示"自然语言处理"的相关结果。除了搜索引擎外, 拼写检查也广泛应用在各种文字处理系统中。

### NLP有希望做那些事？

上面讲了很多应用案例, 其中大部分已经比较成熟甚至已经投入到了商业应用中。下面再罗列一些我认为目前不是很成熟但是很有潜力的NLP技术：

- 句子, 段落的相似性检测：词语的相似度检测已经很成熟, 句子和章节的相似性检测的研究也在进行中。相似性检测有很广的应用空间, 可以用来解决问答论坛上重复问题, 文章抄袭问题等。

- 自动文本摘要：即为文章生成一个简短的总结性段落。当我们写文章时很多人会写一个TLNR（太长不读版）,  文本摘要技术就可以自动为我们生成这个TLNR, 节省我们的时间。在信息爆炸时代, 文本摘要技术有着巨大的潜力。

- 自动问答：该技术的价值无需赘述, 不过目前的问答机器人都只能在特定领域回答一些简单地问题, 通用的问答机器人目前还无法实现, 这将是一个巨大的挑战。该领域的一款落地应用来自Google, 在其邮件应用Inbox中已经开始提供邮件快速回复功能（根据邮件自动生成三个可能的回复供用户选择）, 虽然目前生成的回复都很简短, 但已经有了一定的实用性。


### 总结

上面介绍了几种NLP技术和应用场景, 但是NLP技术涉及的范围远不止这些, 将NLP技术与音频处理、图像处理等技术结合, 又会出现诸如视频字幕生成, 图片描述生成等等有趣的应用。可以说只要有人类, 有语言, 就存在NLP应用的可能性。也正是因为NLP技术涉及范围广泛，才吸引了越来越多企业的关注，并在其之上构建各种智能系统，给我们的生活带来了便利。

最后我想说，如果你读到了这里, 那么不妨花几分钟思考一下, 对于你目前接触到的业务, NLP技术能给客户带来哪些价值呢？
