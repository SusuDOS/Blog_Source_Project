---
title: 神经网络原理&mnist数据集
date: 2019-04-13 11:17:13
categories: mnist数据集
tags: [神经网络传播, mnist数据回归]
---

## 神经网络

- 神经网络迭代原理代码.
- 激活函数使用sigmod.

	```python
	import numpy as np
	
	"""
	简易神经网络：通过不断的调整权重值使得，达到拟合的结果.
	"""
	try:
	    # Python 2
	    xrange
	except NameError:
	    # Python 3, xrange is now named range.
	    xrange = range
	
	
	# sigmod函数，True则返回其导数.
	def sigmod(x, deriv=False):
	    if (deriv == True):
	        return x * (1 - x)
	    return 1 / (1 + np.exp(-x))
	
	
	X = np.array([[0, 0, 1],
	              [0, 1, 1],
	              [1, 0, 1],
	              [1, 1, 1]])
	y = np.array([[0],
	              [1],
	              [1],
	              [0]])
	
	# 随机种子设置：保证random的值恒定.
	np.random.seed(10)
	
	# 随机初始化权重值为：w0:3*4,[-1,1]   w1:4*1,[-1,1]
	w0 = 2 * np.random.random((3, 4)) - 1
	w1 = 2 * np.random.random((4, 1)) - 1
	
	for j in xrange(100000):
	    # 调整10w次.
	    l0 = X
	    # 第一层激活函数执行后的结果(输出矩阵)：5*4
	    l1 = sigmod(np.dot(l0, w0))
	    # 第二层激活函数执行后的结果(输出矩阵)：5*1
	    l2 = sigmod(np.dot(l1, w1))
	
	    # 矩阵差值：残差.
	    l2_error = y - l2
	
	    # 每运行1w窗口显示残差.
	    if (j % 10000) == 0:
	        print("Error:" + str(np.mean(np.abs(l2_error))))
	
	    # 计算l2网络层偏导数，残差*偏导数作为'学习率'=l2_delta.
	    l2_delta = l2_error * sigmod(l2, deriv=True)
	
	    # 5*1 dot (4*1).T==>5*4.
	    l1_error = l2_delta.dot(w1.T)
	
	    # 5*4 * (5*4)==>5*4.此时得到l1'学习率'.
	    l1_delta = l1_error * sigmod(l1, deriv=True)
	
	    # 权重参数调整：4*1.
	    w1 += l1.T.dot(l2_delta)
	    # 权重参数调整：3*4.
	    w0 += l0.T.dot(l1_delta)
	
	```

## 点的生成与拟合.

<font color=blue>生成满足y = a*x + b的数据点.</font>

- 设置数据分布在直线的附近.
	按照实现的速度快慢排序，<font color=green>友情提示：numpy与tensorflow数据类型不兼容，使用`tf.convert_to_tensor()`.</font>	
- code01	
	```python
	import time
	import numpy as np
	import matplotlib.pyplot as plt
	
	"""
	随机生成10000个点，围绕在y=0.1x+0.3的直线周围.
	"""
	
	start = time.clock()
	num_points = 100000
	x = np.random.normal(0.0, 0.6, num_points)
	y = x * 0.1 + 0.3 * np.ones_like(x) + np.random.normal(0.0, 0.03, num_points)
	vectors_set = [x, y]
	
	plt.scatter(x,y,c='r')
	plt.show()
	
	TimeCost = (time.clock() - start)
	print("Time used:", TimeCost)
	Time used: 1.2579034654087218
	```	
- code02	
	```python
	import time
	import numpy as np
	import matplotlib.pyplot as plt
	
	"""
	随机生成100000个点，围绕在y=0.1x+0.3的直线周围.
	"""
	
	start = time.clock()
	num_points = 100000
	vectors_set = []
	for i in range(num_points):
	    x1 = np.random.normal(0.0, 0.6)
	    y1 = x1 * 0.1 + 0.3 + np.random.normal(0.0, 0.03)
	    vectors_set.append([x1, y1])
	
	x_data = [v[0] for v in vectors_set]
	y_data = [v[1] for v in vectors_set]
	
	plt.scatter(x_data, y_data, c='r')
	plt.show()
	
	elapsed = (time.clock() - start)
	print("Time used:", elapsed)
	
	Time used: 3.786280029791725
	```
- 拟合函数：y = f(x) = a*x + b.	
	```python
	import time
	import numpy as np
	import matplotlib.pyplot as plt
	import tensorflow as tf
	
	"""
	随机生成10000个点，围绕在y=0.1x+0.3的直线周围.
	"""
	
	start = time.clock()
	num_points = 10000
	
	# 构造x数组，y数组，并且为y数组添加噪音！
	# 构建数据方法正态分布，均值，标准差.
	x = np.random.normal(0.0, 0.6, num_points)
	y = x * 0.1 + 0.3 * np.ones_like(x) + np.random.normal(0.0, 0.05, num_points)
	
	# 数据集：2*num_points
	vectors_set = [x, y]
	plt.scatter(x, y, c='r')
	plt.show()
	
	# 运行时间计算.
	TimeCost = (time.clock() - start)
	print("Time used:", TimeCost)
	
	# 以下通过这些点的数据，得到一个拟合直线：f(x) = ax+b.
	
	# 构建网络结构.
	# 构建权重参数矩阵：1维矩阵，取值是[-1,1]之间的随机数,数据正态分布.
	W = tf.Variable(tf.random_uniform([1], -1.0, 1.0), name='W')
	# 生成b矩阵:一维零矩阵.
	b = tf.Variable(tf.zeros([1]), name='b')
	# 经过计算得出预估值y
	Forecast_Y = W * x + b
	
	"""
	以预估值y和实际值y_data之间的均方误差作为损失值(残差).
	默认每一列的和求均值，0参数，1参数则为每一行的和求均值.
	"""
	loss = tf.reduce_mean(tf.square(y - Forecast_Y), 0, name='loss')
	
	# 采用随机梯度下优化法：优化参数
	optimizer = tf.train.GradientDescentOptimizer(0.5)
	
	# 目标函数：最小化残差.
	train = optimizer.minimize(loss, name='train')
	
	sess = tf.Session()
	
	# 初始化.
	init = tf.global_variables_initializer()
	sess.run(init)
	
	# 显示：初始化的W和b值.
	print("W =", sess.run(W), "b =", sess.run(b), "loss =", sess.run(loss))
	
	# 执行10次训练：参数调整.
	for step in range(10):
	    sess.run(train)
	    print("第{}训练：".format(step+1),"W =", sess.run(W), "  b =", sess.run(b), "  loss =", sess.run(loss))
	# 日志记录.
	writer = tf.summary.FileWriter("./tmp", sess.graph)
	# 第10训练： W = [0.09806049]   b = [0.30066237]   loss = 0.002453375
	```

## mnist数据集.

### 查看mnist数据集中分好的类.
- 查看手写数字分类数据集-mnist数据集.

	```python
	import numpy as np
	import tensorflow as tf
	import matplotlib.pyplot as plt
	# If the following line reports an error, the second line is used.
	# import input_data
	import tensorflow.examples.tutorials.mnist.input_data as input_data
	
	print("Start Download and Extract MNIST dataset......")
	mnist = input_data.read_data_sets('MNIST_data/', one_hot=True)
	
	# What is the format of mnist data?
	print("What is the format of mnist data?")
	trainimg = mnist.train.images
	trainlabel = mnist.train.labels
	testimg = mnist.test.images
	testlabel = mnist.test.labels
	
	print(" type of 'trainimg' is %s" % (type(trainimg)))
	print(" type of 'trainlabel' is %s" % (type(trainlabel)))
	print(" type of 'testimg' is %s" % (type(testimg)))
	print(" type of 'testlabel' is %s" % (type(testlabel)))
	print(" shape of 'trainimg' is %s" % (trainimg.shape,))
	print(" shape of 'trainlabel' is %s" % (trainlabel.shape,))
	print(" shape of 'testimg' is %s" % (testimg.shape,))
	print(" shape of 'testlabel' is %s" % (testlabel.shape,))
	
	# How does the training data look like?
	print("How does the training data look like?")
	nsample = 5
	# [0,Value(trainimg.shape[0])-1],随机获取nsample个=>np数组！
	randidx = np.random.randint(trainimg.shape[0], size=nsample)
	
	for i in randidx:
	    # 抽取数据=>图像.
	    curr_img = np.reshape(trainimg[i, :], (28, 28))  # 28 by 28 matrix
	    # 获取最大元素的索引,可以看出trainlabel[i, :]是第i图像分类的概率数组！
	    curr_label = np.argmax(trainlabel[i, :])  # Label
	    plt.matshow(curr_img, cmap=plt.get_cmap('gray'))
	    plt.title("" + str(i) + "th Training Data "
	              + "Label is " + str(curr_label))
	    print("" + str(i) + "th Training Data "
	          + "Label is " + str(curr_label))
	    plt.show()
	
	# Batch Learning?
	print("Batch parameter？")
	batch_size = 4096
	batch_xs, batch_ys = mnist.train.next_batch(batch_size)
	print("type of 'batch_xs' is %s" % (type(batch_xs)))
	print("type of 'batch_ys' is %s" % (type(batch_ys)))
	print("shape of 'batch_xs' is %s" % (batch_xs.shape,))
	print("shape of 'batch_ys' is %s" % (batch_ys.shape,))	
	```
### mnist数据集

- 单层神经网络-mnist数据集的逻辑回归.
- 利用0-9数字的手写体进行数据的训练.
- 训练组数：55000，训练集：55000 * 784，标签集：55000*10
- 测试组：10000，测试集：10000 * 784，标签集：55000*10
	```python
	import time
	import tensorflow as tf
	import numpy as np
	# import input_data
	from tensorflow.examples.tutorials.mnist import input_data
	
	start = time.clock()
	mnist = input_data.read_data_sets("MNIST_data/", one_hot=True)
	train_set = mnist.train.images
	train_label = mnist.train.labels
	test_set = mnist.test.images
	test_label = mnist.test.images
	
	# 矩阵结构显示：
	print("train_set.shape:", train_set.shape)
	print("train_label.shape:", train_label.shape)
	print("test_set:", test_set.shape)
	print("test_label:", test_label.shape)
	
	# label结构显示.
	print("train_set[0]:", train_label[0])
	
	# 构建train训练矩阵.
	input = tf.placeholder(tf.float32, [None, 784])
	output = tf.placeholder(tf.float32, [None, 10])
	
	weight = tf.Variable(tf.random_normal([784, 10], 0, 0.0001), name="weight")
	bias_term = tf.Variable(tf.random_normal([10], 0, 0.0001), name="bias_term")
	
	# 激活函数.
	activate_function = tf.nn.softmax(tf.matmul(input, weight) + bias_term)
	"""
	cost_fuction = tf.reduce_mean(-tf.reduce_sum(output
	                                             * tf.log(activate_function), reduction_indices=1))
	关于损失函数的描述：
	    output * tf.log(activate_function>得到一个:None*10矩阵.
	    reduc_sum([None*10],1)，把每列的数据加起来.None*(1)矩阵,注意表述.
	    reduc_mean()，求取平均.
	    cast = train_label - activate_function*train_label
	    求最小值：根据等式基本性质，等价于求解min(cast =activate_function*train_label)
	"""
	
	# cost_fuction = tf.reduce_mean(-tf.reduce_sum(output * tf.log(activate_function), 1))
	# 上行替换为下行亦可，本质上是了解reduce的具体意义.
	cost_fuction = tf.reduce_mean(-output * tf.log(activate_function))
	learning_rate = 0.01
	
	# 训练的目标函数：利用随机梯度下降参数调整使得，cost_fuction最小.
	optm_fuction = tf.train.GradientDescentOptimizer(
	    learning_rate).minimize(cost_fuction)
	# 预测准确性操作，True/Fals.
	predict_op = tf.equal(tf.argmax(activate_function, 1), tf.argmax(output, 1))
	# True/False对Float数据类型的变换.
	# 为什么直接用mean函数即可，因为每一行仅有一个元素为1，其余为0，求和/行号和=精度.
	accuracy_rater = tf.reduce_mean(tf.cast(predict_op, "float32"))
	# 数据的初始化.
	init = tf.global_variables_initializer()
	
	"""
	测试显示效果.
	sess = tf.InteractiveSession()
	# sess = tf.Session()
	
	arr = np.array([[31, 23,  4, 24, 27, 34],
	                [18,  3, 25,  0,  6, 35],
	                [28, 14, 33, 22, 20,  8],
	                [13, 30, 21, 19,  7,  9],
	                [16,  1, 26, 32,  2, 29],
	                [17, 12,  5, 11, 10, 15]])
	tf.rank(arr).eval()
	#tf.shape(arr).eval()
	#tf.argmax(arr, 0).eval()
	# 0 -> 31 (arr[0, 0])
	# 3 -> 30 (arr[3, 1])
	# 2 -> 33 (arr[2, 2])
	tf.argmax(arr, 1).eval()
	5 -> 34 (arr[0, 5])
	5 -> 35 (arr[1, 5])
	2 -> 33 (arr[2, 2])
	"""
	# 所有图片完成的迭代次数.
	training_epochs = 400
	# 每一次训练数据的大小，这里是输入图片的个数，等价于行的行数.
	# 取值小一些，收敛速度相对而言快一些.本质上是每运行一次所有图片,调整参数次数更多.
	# 一次性32 64 128皆可.
	batch_size = 32
	# 显示步进.
	display_step = 10
	# SESSION
	sess = tf.Session()
	sess.run(init)
	
	# StartLearning.
	for epoch in range(training_epochs):
	    avg_cost = 0.
	    num_batch = int(mnist.train.num_examples / batch_size)
	    # num_batch走完一轮表示，55000图片跑完一遍,尾部有省略部分数据.。
	    for i in range(num_batch):
	        # input,output.
	        batch_xs, batch_ys = mnist.train.next_batch(batch_size)
	        sess.run(optm_fuction, feed_dict={input: batch_xs, output: batch_ys})
	        feeds = {input: batch_xs, output: batch_ys}
	        """
	        一次进入的64张图，得到的损失函数/num_batch=？只是表示一种度量对运行结果没有影响.
	        但是就具体意义而言，不严谨.
	        avg_cost += sess.run(cost_fuction, feed_dict=feeds) / num_batch batch_size
	
	        此时的损失函数代表的是：64*1个图片的残差，64*2...64*num_batch的残差
	        运行完成所有数据一次，weight，bias_term参数保存，重新构建残差，继续迭代训练.
	        """
	        avg_cost += sess.run(cost_fuction, feed_dict=feeds) / batch_size
	   
	    if epoch % display_step == 0:
	        feeds_train = {input: batch_xs, output: batch_ys}
	        feeds_test = {input: mnist.test.images, output: mnist.test.labels}
	        train_accuracy_rate = sess.run(accuracy_rater, feed_dict=feeds_train)
	        test_accuracy_rate = sess.run(accuracy_rater, feed_dict=feeds_test)
	        print("Epoch: %03d/%03d cost: %.9f train_accuracy_rate: %.3f test_accuracy_rate: %.3f"
	              % (epoch, training_epochs, avg_cost, train_accuracy_rate, test_accuracy_rate))
	TimeCost = (time.clock() - start)
	print("Time used:", TimeCost)
	print("DONE")
	
	"""
	每一次输入图像的多少对收敛速度影响，表现在：
	输入的图像数量增加，出现train精度开始小于test数据组中的精度.(以16为标准说明.)
	训练时间在小输入组表现高于大输入组，时间呈负相关.	
	"""
	
	```

### mnist两层神经网络实现.

- 为了减少数据的运算量，第一步开始大幅度减少参数个数.
- 第一层隐藏层W:128;第二层隐藏层W:64.
- ` 784 * 128` ，`64 * 10`隐藏层矩阵大小.
- 显示效果(100次.)：
![捕获.PNG](https://i.loli.net/2019/04/16/5cb5eae0c39e5.png)

- 代码如下：
	```python
	import tensorflow as tf
	import numpy as np
	import matplotlib.pyplot as plt
	import tensorflow.examples.tutorials.mnist.input_data as input_data
	
	mnist = input_data.read_data_sets('MNIST_data/', one_hot=True)
	
	w_hidden_1 = 128
	w_hidden_2 = 64
	n_input = 784
	n_classes = 10
	
	x = tf.placeholder('float32', [None, n_input], name='x')
	y = tf.placeholder('float32', [None, n_classes], name='y')
	
	stddev = 0.01
	
	weights = {
	    'w1': tf.Variable(tf.random_normal([n_input, w_hidden_1], 0, stddev=stddev)),
	    'w2': tf.Variable(tf.random_normal([w_hidden_1, w_hidden_2], 0, stddev=stddev)),
	    'w_out': tf.Variable(tf.random_normal([w_hidden_2, n_classes], 0, stddev=stddev))
	}
	biases = {
	    'b1': tf.Variable(tf.random_normal([w_hidden_1], 0, stddev=stddev)),
	    'b2': tf.Variable(tf.random_normal([w_hidden_2], 0, stddev=stddev)),
	    'b_out': tf.Variable(tf.random_normal([n_classes], 0, stddev=stddev))
	}
	
	
	def multilayer_perceptron(x, weights, biases):
	    hidden_1 = tf.nn.sigmoid(tf.add(tf.matmul(x, weights['w1']), biases['b1']))
	    hidden_2 = tf.nn.sigmoid(tf.add(tf.matmul(hidden_1, weights['w2']), biases['b2']))
	    return (tf.matmul(hidden_2, weights['w_out']) + biases['b_out'])
	
	
	pred = multilayer_perceptron(x, weights, biases)
	
	cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(labels=y, logits=pred))
	opt = tf.train.GradientDescentOptimizer(learning_rate=0.01).minimize(cost)
	corr = tf.equal(tf.argmax(pred, 1), tf.argmax(y, 1))
	accr = tf.reduce_mean(tf.cast(corr, 'float32'))
	
	init = tf.global_variables_initializer()
	
	train_epoches = 100
	batch_size = 64
	display = 1
	sess = tf.Session()
	sess.run(init)
	for epoch in range(train_epoches):
	    avg_cost = 0
	    batch_number = int(mnist.train.num_examples / batch_size)
	    for i in range(batch_number):
	        batch_x, batch_y = mnist.train.next_batch(batch_size)
	        feeds = {x: batch_x, y: batch_y}
	        sess.run(opt, feed_dict=feeds)
	        avg_cost += sess.run(cost, feed_dict=feeds)
	    avg_cost = avg_cost / batch_number
	    # 显示包括第一次执行完成后结果.
	    if (epoch) % display == 0:
	        print("Epoch: %04d/%04d cost: %.9f" % (epoch, train_epoches, avg_cost))
	        feeds = {x: batch_x, y: batch_y}
	        train_acc = sess.run(accr, feed_dict=feeds)
	        print("TRAIN ACCURACY: %.4f" % (train_acc))
	        feeds = {x: mnist.test.images, y: mnist.test.labels}
	        test_acc = sess.run(accr, feed_dict=feeds)
	        print("TEST ACCURACY: %.4f" % (test_acc))
	        print()
	print("OPTIMIZATION FINISHED")
	```
