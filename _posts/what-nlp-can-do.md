上次博客大赛，我简单介绍了自然语言处理都在哪些场景得到应用。这次我们从技术角度分析，哪些东西是我们TW在做自然语言处理项目时可以做到的、可能做到的。

1. NLP 是什么，他的历史？主要包括自然语言的理解，和自然语言生成，

3. 由于自然语言处理太难了，人们把它分成了很多个子任务，子任务有哪些呢？关键的包含。。。。。

2. NLP现状，和自然语言能处理对比。NLP为什么难？


4. 

极大工具，allenNLP，stanfordNLP

BERT EMLO

现有任务准确度，中文上面的准确度。


大家都知道，自然语言处理的应用远不及图像处理来的成熟。图像处理已经走进大街小巷，人脸识别成熟度非常高，高到连过海关都可以自动化，给大家带来了极大地方便。反观NLP，除了每天调戏的Siri，小爱等人工智障，其他的应用似乎离我们都比较远。

有人可能会说，语音助手已经很成熟了。的确，在预先定义好的场景下，语音助手已经能达到很高的准确度。但是对预定场景外的内容，当前的语音助手基本无能为力。

语言比图像复杂得多，具有很高德不确定性，现阶段的自然语言处理还远达不到成熟的地步。在不成熟的技术上，我们能为客户提供哪些价值呢？由于我们的业务模式，一般很难长时间做研究性工作；我们更多的是把现有的技术与客户业务场景相结合，为客户创造价值。那现有技术是什么？我任务就是世面上已有的一些类库和api；

自然语言处理太复杂了，怎么办？人们就把复杂任务分解，从简单的开始解决。


命名实体识别（Named entity recognition）：识别文本中的实体，一般来说实体主要指人名，地名，时间，日期，公司名，数字等。




Spacy功能
Named entity recognition，经典功能，研究的也比较深入。
Pre-trained word vectors  词向量
Easy deep learning integration 
Part-of-speech tagging - POS part of speech 词性标注
Labelled dependency parsing  句法分析
Syntax-driven sentence segmentation
Built in visualizers for syntax and NER
Convenient string-to-hash mapping
Export to numpy data arrays
Efficient binary serialization
Easy model packaging and deployment
State-of-the-art speed
Robust, rigorously evaluated accuracy


pytext

AllenNLP

Gensim，非监督文本建模，训练词向量

CoreNLP

指代消解


scikit-learn，文本分类，内置词袋模型，


首先是最简单的分词，词性标注，


bert  1. 事件分类。 2. 情感分类-》上涨下跌中性。 
聚类， 主题模型 两层聚类。