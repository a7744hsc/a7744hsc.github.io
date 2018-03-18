---
layout: post
title:  "How to learn Spark from scratch"
date:   2018-03-15 18:00:00 +0800
categories: Machine Learning
---

Recently, I was asked to join a spark project. But unfortunately，I knew nothing about spark and scala then. It was a really a challenge for me to learn Spark in a very short time and this is a review of my learning path. If you are in a similiar situation, this article may save you some effort.

## Which programming language should I use?

Spark is written in Scala and most users wrting spark program with `Scala`. But spark also support other languages like `Python`, `Java` and `R`. I was trying to learn PySpark since I have some experience on Python, but I choosed Scala at last. The main reason are listed below:

1. Since Scala is navive supported language, new scala features are always added to scala first.

2. Many ideas in spark are borrowed from scala, the RDD is similiar with the scala collections. Learning scala, along with the functional thinking, is very helpful for learning Spark.

3. Even though pysark gained more and more attention recently, there are more resources about spark on scala.

It is always a trade-off when you make some decision, but I suggest scala if you are not just have a try. A good thing is that the API for python and scala is similiar, so you can easily move to another language.

## What I want to know? 

When I start to learn something, I always try to make myself very clear that what is the key point I really want to know. This will give me a clear overview of the new tech but not all the details of API.

1. Basic spark grammer which is enough for me to start programming.
2. What is spark, what problem can it solve? 
3. What can spark do, what is the thing that spark not good at.

## Where should I start to learn scala?

Scala is quite popular nowadays and you can find a lot resourses online. I suggest learn it systemeticly through a book or an online course but some random blogs if you are new to functional-programming. That is because the functional concept behind grammar is more important and harder to learn.

My choice is start with a book. I prefer to read a book when learn a new programming language, because it is more structured. It is alsovery convinience for reference. The book I read is `Functional Programming in Scala` from `Paul Chiusano` and `Runar Bjarnason`, which is a great one include not only language basics but also include why the language is designed like that. Since my ultimate goal is spark and the limited time, I only read the first 4 chapters for the basics. I highly recommend this book if you are also a novice of functional programmer.

## Spark learning materials.

Spark have very good [official start document][spark-tutorial] and there is also a [Chinese version] [spark-chinese-tutorial]. I have gone through the tutorials and quickly created my first spark program. Threre is a series of tutorials on the official website with a hight quality.

If you are not into reading, some MOOCs are also available for you. [This][cousera-spark] is a good one from cousera which teach the details of RDD and Spark-SQL in an easy to understand way. One downside is this course doesn't include MLlib and spark streaming, which are both important parts of spark.

## Other things may be useful for your learning journey.

1. Set up spark environment is not hard, just download the pre-compiled binary file and unzip them as described [here][spark-install], But this is only a local version. An alternative way is writing your spark code on [databricks](https://www.databricks.com), it is an online data analysis platform support spark, you can start from a free plan.

2. Spend some time on learning sbt, you will use it all the time in scala project.


## Scala key points

Coming soon... ^-^



## Spark Key points


<!-- RDD DataFrame DataSet

    1. Spark 如果读取本地文件，则所有worker的对应目录上都应该有该文件。
    2. 最好传递匿名函数或单例函数给spark，而不要传递实例函数（浪费资源）。
    3. 

并行计算和分布式计算主要区别是分布式计算需要考虑网络延迟。
spark使用函数式编程理念来容错，不用写入硬盘，只有在错误恢复时才有网络传输，所以比hadoop快。
RDD transformation 是lazy的
RDD 里没有 foldleft，改变type只能是aggregate。
reduceByKey 比 groupByKey更高效
shuffle后的操作最好能persist，防止多次执行shuffle，有些操作会改掉partition。
sparkSQL更加结构化，便于spark优化性能。 将关系型数据库和spark结合起来。
Dataframe is RDD with schema，but it is untyped -->





[spark-tutorial]: https://spark.apache.org/docs/latest/quick-start.html
[spark-chinese-tutorial]: http://spark.apachecn.org/docs/cn/2.2.0/quick-start.html
[cousera-spark]: https://www.coursera.org/learn/scala-spark-big-data
[spark-install]: https://spark.apache.org/docs/latest/