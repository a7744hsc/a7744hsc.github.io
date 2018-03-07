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

[Slide 6](http://web.stanford.edu/class/cs20si/lectures/slides_06.pdf)
===========

卷积，池化，特征可视化
特征翻转，纹理合成，风格迁移
CNN：CS231n， lecture7
Inception model details

（guided） backprop，将权重固定，计算输入的变化对输出地影响。
guided backprop,只把正的误差向前传播
GAN: gradient ascent network  生成图像
feature inversion 特征反转，根据特征生成图片
texture synthesis， 文理合成


[Slide 7](http://web.stanford.edu/class/cs20si/lectures/slides_07.pdf)
===========
tf.nn.conv2d 中 的stride 是一个长度为4的list，分别代表。[batch, height, width, channels]

reduce_sum and reduce_mean 做loss的区别

初始化  truncated_normal   random_normal     mean, std   对训练的影响。


[Slide 9](http://web.stanford.edu/class/cs20si/lectures/slides_09.pdf)
===========
1. io 慢与处理速度，所以多线程io，所以用queue
2. python jil(全局解释器锁) 多线程支持一般  难跨cpu
3. QueueRunner  创建多个线程来同时对queue进行写入； coordinater用于管理线程（停止，抛出异常）   在python中帮助实现多线程.  
用来手动停止线程，避免被python强行杀掉：
```
    coord.request_stop()
    coord.join(enqueue_threads)
```
4. 试一下 fifoqueue   查一下paddingqueue（支持可变形状的element）
5. reader vs   feed dict        queue with feed dict?    reader with feed dict?
6. TFRecord 使用

[Slide 10](http://web.stanford.edu/class/cs20si/lectures/slides_11.pdf)
==========




[Slide 13](http://web.stanford.edu/class/cs20si/lectures/slides_13.pdf)
==========
attention model in seq2seq(seq2seq with attention)
自动找关注点

[Bucketing]   (group sequences of similiar length together)
tf.contrib.training.bucket_by_sequence_length
截断，训练时减少补零的个数
tensorflow 老版本特有，现在已经不用了，用dynamic_rnn

Sampled Softmax

