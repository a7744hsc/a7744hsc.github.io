[Slide 1](http://web.stanford.edu/class/cs20si/lectures/slides_01.pdf)
===========



tensorboard demo:
```
a = tf.constant(2,name='a')
b = tf.constant(3,name='b')
x = tf.add(a, b)
with tf.Session() as sess:
# add this line to use TensorBoard.
    writer = tf.summary.FileWriter('./graphs', sess.graph)
    print (sess.run(x))
writer.close()
```


tf.reset_default_graph()  #reset default grapy

[Slide 2](http://web.stanford.edu/class/cs20si/lectures/slides_02.pdf)
===========


veryfy_shape
a = tf.constant(2,shape=[2,3],verify_shape=True)

broadcast
random shuffle multinomial random_gamma

不要用constant存图片或者其他数据，用reader或者variable

lazy loading 
```
x = tf.Variable(10, name='x')
y = tf.Variable(20, name='y')

with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    for _ in range(10):
        sess.run(tf.add(x, y))
    writer = tf.summary.FileWriter('./my_graph/l2', sess.graph)
    print (tf.get_default_graph().as_graph_def())
    writer.close()
```

使用 property 组织tensorflow model
https://danijar.com/structuring-your-tensorflow-models/

sess.graph.as_graph_def()  #print graph def
sess = tf.InteractiveSession() #useful for play tensorflow in command line.

[Slide 3](http://web.stanford.edu/class/cs20si/lectures/slides_03.pdf)
===========


[Slide 4](http://web.stanford.edu/class/cs20si/lectures/slides_04.pdf)
===========


SVD  similiar value decomposition

word2vec   skip gram   vs   cbow
NCE vs negetive sample.

[Slide 5](http://web.stanford.edu/class/cs20si/lectures/slides_05.pdf)
===========

gradient， reference to cs231n lecture 4
tf.gradients演示

模型组织,OOP  
`04_word2vec_visualize.py`

summary 记录信息 然后在tensorboard中展示

random 演示：
```
    import tensorflow as tf
    sess = tf.InteractiveSession()
    a= tf.random_normal([],seed=10)
    b= tf.random_normal([],seed=10)
    a.eval()
    b.eval()
```

File reader

logits regression

