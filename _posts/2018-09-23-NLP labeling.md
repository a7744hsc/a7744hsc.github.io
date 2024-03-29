---
layout: post
title:  "自然语言标注"
date:   2018-09-23 17:00:00 +0800
categories: Nature Language Processing
---
自然语言处理（NLP）是机器学习领域非常有挑战的一个分支，到目前为止NLP相关的应用仍然不多（或者不成熟）。但是，当我们学习自然语言处理课程或者做实验的时候却常常能得到不错的结果。那么“实验”和应用之间的差距在哪里呢？回想一下，当我们学习NLP的时候一定会学到各种算法，然后应用这些算法在标准语料上解决一些问题（如分类，标注等），使用适当的算法在标准数据集经过简单调优就能得到不错的效果。但是，当我们为自己的模型开心时却很少关注所用的标准数据集。事实上，一个标注良好的数据集对模型的准确度非常重要，而在实际应用中，我们时长会发现语料往往存在这样或者那样的问题。而一旦语料存在问题，那么任何算法都不可能取得好的结果，因为“老师教的本身就有问题”。

所以对于NLP工程来说，良好的语料标注时项目成功的基础。下面就总结出了一些语料标注的方法（部分内容来自于`Natural Language Annotation for machine learning`这本书）。

### 语料标注的注意事项：
1. 语言学十分复杂，主要包括以下几个分支：句法学，语义学，形态学（词跟，前缀后缀等），音系学，语音学，词汇，话语分析，语用学，篇章结构分析。了解这些分支有助于指定标注方式。

2. 目前NLP可以处理的任务主要包含以下几个大类：问答系统，文档摘要，机器翻译，语音识别，文档分类等。明确使用语料的算法有助于建立语料标注的标准和模型。

3. 对于自然语言处理任务，主要包含下面几个步骤：建模（标注体系），标注，训练，测试，评价，修改。语料标注是其中建模和标注两部。语料标注过程常常需要在建模和标注之间就行迭代（很难一次性建立完美的标注模型）。

4. 标注数据的一致性非常重要，应该建立标注标准，标注方式需要细化，否则很难再不同标注人员间统一，下面是一个对机构进行标注的例子, 三种标注方式都比较合理，但却存在很大不同：

    - 东英格兰[QBC制造]公司
    - 东英格兰[QBC制造公司]
    - [东英格兰QBC制造公司]

    标注不一致也是语料标注最主要的问题，严重影响语料质量，该问题在标注大型语料库时尤为明显，需要谨慎应对。

5. 标注前应明确定义标注目标（用途，标注产出，语料来源和标注方式），单个标注任务应该足够小（大的标注任务可以分解成多个子任务），这样能降低标注的难度从而提高标注正确率。在标注过程中，应尽可能使用自动化方式减少标注人工作量。

6. 语料库选取应注意代表性和平衡性。代表性指语料库能大体代表其背后的数据。例如，要分析中文新闻，语料库就需要尽可能的覆盖多个新闻源和新闻类别，而不是只用单一新闻源构建语料库；而平衡性则要求语料库中的类别组成应该与真实数据一致。

7. 对于大型标注任务来说，有必要建立文本标注的模型，模型可以使用DTD语言建立，也可以使用现有的一些标准模型。不过个人认为对于小型任务，过于复杂的标准化模型反而增加了标注的复杂度。

8. 正式开始标注前需要建立标注的规范，如单标签标注、多标签标注；内嵌式标注、分离式标注。对于大型项目，应编写标注说明手册以降低标注人之间的差异

9. 标注工具的选择，市面上有多种标注工具可供选择，例如MAE， Callisto，Brandeis Annotation Tool，Prodigy（收费）等。如果没有使用过任何工具，那么MAE可能是一个不错的开始。

10. 当有多人对相同数据进行标注时，可以通过检查标注一致性来判断标注的质量。标注一致性的计算方式包括：IAA 或者 cohen Kappa，Fleiss Kappa。

11. 标注完成后应该有一个审核的过程，审核过程最好由参与制定标注标准的人来执行。需要注意的是审核过程会十分的耗时（甚至有可能话费多于标注过程的时间），需要合理安排资源。

总得来说标注是一个比大多数人想象中更复杂的工程，在实际实施中总汇遇到各种各样的问题。如果没有足够的经验，可以先对少量数据进行试错再大规模执行，以避免走太多弯路。而与此同时标注又是机器学习中十分重要的一环，其对结果的影响不亚于模型构建和调优，应该给予更多的重视。



