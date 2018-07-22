---
layout: post
title:  "Flask Blueprint"
date:   2018-07-18 11:00:00 +0800
categories: Python
---

Python web开发一直以其高效，快速的特点在后端开发中占有一席之地。而Flask又是轻量级Web框架中的佼佼者。本文主要介绍Flask中的Blueprint特性。

## 为什么要用Blueprint
Python 提倡简洁，快速的编码风格。所以Python的类库设计也力图简洁，Flask也是这样。而Python项目在组织结构上也十分灵活，没有严格要求，所以Flask的HellowWorld只需要下面五行代码，一个文件。：

```
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"
```

和其他web框架相比，这是一个非常简洁的Hello word示例，没有复杂的层级结构和配置文件。而用Flask开发的简单Restful API服务也可以轻易的包含在一个文件当中。下面是一个对User进行增删改查的示例，他可以很好的组织在一个文件中：

```
from flask import Flask
import os

app = Flask(__name__)

@app.route("/user", methods=["POST"])
def add_user():
    pass

@app.route("/user", methods=["GET"])
def get_user():
    pass

@app.route("/user/<id>", methods=["GET"])
def user_detail(id):
   pass

@app.route("/user/<id>", methods=["PUT"])
def user_update(id):
    pass

# endpoint to delete user
@app.route("/user/<id>", methods=["DELETE"])
def user_delete(id):
    pass

if __name__ == '__main__':
    app.run(debug=True)
```

可是后来你不但要提供User的增删改查，你还要提供商品的操作，然后加上权限控制....，最后你的APP很可能超过1000行，逻辑也变得越来越复杂，维护一个几百上千行代码的文件一定是你不想要做的，所以这时候你可能会想办法把把实现代码抽到单独的文件中，只把路由放在这里；再或者建立多个Flask app分别负责单独的任务....当你在不断探索不同的方法的时候，Flask已经提供了官方的解决方案，那就是Flask的Blueprint。

## blueprint 简介

蓝图是Flask里面用来模块化大型项目的一个特性，他十分易用，而且提供了[中文文档][blueprint-chinese]。总的来说Flask有以下几个特点：

1. Blueprint不是一个应用，而是一个模板，你可以在app上多次注册同一个Blueprint。
2. Blueprint在使用上接近于一个Flask实例，有一些类似的语法可用。
3. Blueprint可以注册到一个子域名或者后缀上。
4. Blueprint也可以用来提供资源。


## 我的理解

Python是一门自由的语言，没有其他语言中众多的限制，因此基于Python的web框架Flask也可以通过多种多样的方式进行组织（而不像Spring中有着严格的约束）。但是自由可能会带来混乱，尤其是在大型项目中更是如此，而Blueprint就是大型Flask项目提供了一种参考的组织方式。所以对于大型Flask项目，使用Blueprint来对项目进行模块化的管理是一个不错的选择。

## 一些使用了 blueprint 和 flask app demo

Blueprint本身并不复杂，但是引入Blueprint会同时引入一些代码结构上的规范，下面提供了一些不错的的Demo供参考，可以帮助初学者更好的了解一个使用Blueprint的项目长什么样子：

1. [Flask Restful Demo](https://github.com/Leo-G/Flask-SQLALchemy-RESTFUL-API):一个简单地Restful 的demo。

2. [Flask-Scaffold](https://github.com/Leo-G/Flask-Scaffold): 一个Flask写的Restful API，比较完善，提供web管理界面。





[blueprint-chinese]:http://dormousehole.readthedocs.io/en/latest/blueprints.html