---
layout: post
title: "How to learn Spark from scratch & Key points of Spark"
date:  2018-03-15 18:00:00 +0800
categories: Machine Learning
---

Recently, I was asked to join a spark project. But unfortunately，I knew nothing about spark and scala then. It was a really a challenge for me to learn Spark in a very short time and this is a review of my learning path. If you are in a similiar situation, this article may save you some effort.

## Which programming language should I use?

Spark is written in Scala and most users wrting spark program with `Scala`. But spark also support other languages like `Python`, `Java` and `R`. I was trying to learn PySpark since I have some experience on Python, but I choosed Scala at last. The main reasons are listed below:

1. Since Scala is native supported language, new spark features are always added to scala API first.

2. Many ideas in spark are borrowed from scala, the RDD is similiar with the scala collections. Learning scala, along with the functional thinking, is very helpful for learning Spark.

3. Even though pysark gained more and more attention recently, there are more resources about spark on scala.

It is always a trade-off when you make some decision, but I suggest scala if you are not just have a try. A good thing is that the API for python and scala is similiar, so you can easily move to another language. You can find more details about the comparison between Spark on Scala and Python [here](https://www.datacamp.com/community/tutorials/apache-spark-python#language)

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

## Spark Key Learnings

After go through the tutorials and books, here are some key points I learned. This can be used as a quick learning material or a checkpoint of your learning.

1. Spark can read files from HDFS or local file system. If you read file from local system, you need to put the file in the same path for each of the workers.
2. Pass a class method or anonymous method to spark instead of instance method. Because spark will pass the whole instance to each worker and this will waste resources.
3. When programming with Spark, the major concern is network latency, so we want to minimize the data transformation between workers.
4. Spark implement fault tolerence with idea from functional programming. RDD in spark is immutable, when a worker is down spark can recalculaete the RDD based on the rule and original RDD.
5. Transformations in RDD are lazy which means they won't be calculated until an action is called.
6. When programming with RDD, we should think about reduce the shuffule operation since it is very time comsuming. We can achieve it by doing manually partition or creating narrow dependency RDDs.
7. Differences between RDD, DataFrame and DataSet. RDD have the biggest flexibility but you need to take care of the performance. DataFrame and DataSet are structured interface build on the top of RDD, operations on them can be optimized by spark to achieve a relative higher performance. DataFrame is an untyped version of DataSet represented by DataSet[Rows]
8. In spark, each task include several stages and each stage include several jobs. A stage include one shuffle operation, a job include one spark action.
9. Spark streaming split the real time data stream into small data batches and perform operations on each of these data batches. So it can only achieve an at least 100ms delay(There are improvement in recent versions).




[spark-tutorial]: https://spark.apache.org/docs/latest/quick-start.html
[spark-chinese-tutorial]: http://spark.apachecn.org/docs/cn/2.2.0/quick-start.html
[cousera-spark]: https://www.coursera.org/learn/scala-spark-big-data
[spark-install]: https://spark.apache.org/docs/latest/
