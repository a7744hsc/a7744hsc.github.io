1.tf 包含哪些部分

2. 恼人的High level api

3. tf data 模块 和 TFRecord
https://www.tensorflow.org/alpha/tutorials/load_data/images#pipe_the_dataset_to_a_model
tf data包的目的是解决当利用GPU训练模型时，数据预处理（包括文件的读取）和模型计算之间并行性，使得预处理和训练过程能并行进行，提升学习效率。 tf.data提供了相对简单的API帮我们快速实现这种并行化。还可以实现缓存
tf.data.Dataset是tf.data下的一种类型，可以用来管理训练数据，他提供了repeat，shuffle和buffer的功能

- Dataset可以保存成TFRecord，也可以从TFRecord读取，效率比读取原始文件有提升。

- tf.data.Datasets , 包含一些可以直接使用的数据集。


4. TF Hub



all_image_paths = list(data_root.glob('*/*'))


inceptionResnetV2 preprocessing！！！！！！！