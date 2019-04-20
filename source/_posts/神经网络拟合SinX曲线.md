---
title: SinX函数曲线的拟合/逼近.
date: 2019-04-10 15:17:13
categories: 曲线拟合
tags: [SinX拟合, 神经网络]
---

## SinX函数曲线的拟合/逼近.

<font color = blue>说明：原代码来自罗冬日-tensorf实战书籍代码，且该代码选自googl官方案例.</font>

- 网络层次结构：W 为16维，depth为3，也就说说合计参数48个.
- 采用随机梯度优化，但是原书中提示实际运用中Adam(亚当)优化效果更好：收敛速度更快.
- 代码在原代码有少部分修改.
- 运行结果如下：起始/5w次后的结果.
![00001.PNG](https://i.loli.net/2019/04/16/5cb5cecb38823.png)
![00002.PNG](https://i.loli.net/2019/04/16/5cb5cecb38f10.png)
- 代码.
	```python
	# coding=utf-8
	import tensorflow as tf
	import numpy as np
	import matplotlib.pyplot as plt
	import pylab
	
	'''
	  用TensorFlow来拟合一个正弦函数.
	  代码选自：罗冬日-tensorflow实战.
	  罗冬日代码改写自google官方案例.
	'''
	#分配第一块显卡，若是在tensorflow-gpu下默认使用的显卡，可以不添加改行.
	# with tf.device("/gpu:0"):
	
	# 改行用于每次运行都重置变量，否则同名变量无法运行.
	tf.reset_default_graph()
	
	#定义显存分配百分比，貌似不准确，有点误差..
	config = tf.ConfigProto()
	# %matplotlib qt
	# config.gpu_options.per_process_gpu_memory_fraction =  0.6 # 分配60%显存.
	config.gpu_options.allow_growth =  True # 自动分配.
	with tf.Session(config = config) as sess:
	    def draw_correct_line():
	        '''
	          绘制标准的sin曲线
	        '''
	        x = np.arange(0, 2 * np.pi, 0.01)
	        x = x.reshape((len(x), 1))
	        y = np.sin(x)
	
	        pylab.plot(x, y, label='standard sin:')
	        plt.axhline(linewidth=1, color='r')
	
	
	    def get_train_data():
	        '''
	          返回一个训练样本(train_x, train_y),
	          其中train_x是随机的自变量， train_y是train_x的sin函数值
	        '''
	        train_x = np.random.uniform(0.0, 2 * np.pi, (1))
	        train_y = np.sin(train_x)
	        return train_x, train_y
	
	    # tf.reset_default_graph()
	
	
	    def inference(input_data):
	        '''
	        定义前向计算的网络结构.
	        Args:
	          输入的x的值，单个值.
	        网路层的weight和bias初始化为：均值为0，方差为1.
	        '''
	
	        with tf.variable_scope('hidden1'):
	            # 第一个隐藏层，采用16个隐藏节点
	            weights = tf.get_variable("weight", [1, 16], tf.float32,
	                                      initializer=tf.random_normal_initializer(0.0, 1))
	            biases = tf.get_variable("biase", [1, 16], tf.float32,
	                                     initializer=tf.random_normal_initializer(0.0, 1))
	            hidden1 = tf.sigmoid(tf.multiply(input_data, weights) + biases)
	
	        with tf.variable_scope('hidden2'):
	            # 第二个隐藏层，采用16个隐藏节点
	            weights = tf.get_variable("weight", [16, 16],  tf.float32,
	                                      initializer=tf.random_normal_initializer(0.0, 1))
	            biases = tf.get_variable("biase", [16], tf.float32,
	                                     initializer=tf.random_normal_initializer(0.0, 1))
	
	            mul = tf.matmul(hidden1, weights)
	            hidden2 = tf.sigmoid(mul + biases)
	
	        with tf.variable_scope('hidden3'):
	            # 第三个隐藏层，采用16个隐藏节点
	            weights = tf.get_variable("weight", [16, 16],  tf.float32,
	                                      initializer=tf.random_normal_initializer(0.0, 1))
	            biases = tf.get_variable("biase", [16], tf.float32,
	                                     initializer=tf.random_normal_initializer(0.0, 1))
	            hidden3 = tf.sigmoid(tf.matmul(hidden2, weights) + biases)
	
	        with tf.variable_scope('output_layer'):
	            # 输出层
	            weights = tf.get_variable("weight", [16, 1],  tf.float32,
	                                      initializer=tf.random_normal_initializer(0.0, 1))
	            biases = tf.get_variable("biase", [1], tf.float32,
	                                     initializer=tf.random_normal_initializer(0.0, 1))
	            output = tf.matmul(hidden3, weights) + biases
	
	        return output
	
	
	    def train():
	        # 学习率
	        learning_rate = 0.01
	        x = tf.placeholder(tf.float32)
	        y = tf.placeholder(tf.float32)
	
	        net_out = inference(x)
	
	        # 定义损失函数的op
	        loss_op = tf.square(net_out - y)
	
	        # 采用随机梯度下降的优化函数
	        opt = tf.train.GradientDescentOptimizer(learning_rate)
	        # opt = tf.train.AdamOptimizer.(learning_rate)
	
	        train_op = opt.minimize(loss_op)
	
	        init = tf.global_variables_initializer()
	
	        with tf.Session() as sess:
	            sess.run(init)
	            print("start training....")
	            # 训练次数，实际上在3w次后几乎完全拟合，矩阵/节点的大小是可以提高拟合速度的，但是效果明显低于不入拟合次数。
	            train_times = 1000000
	
	            for i in range(train_times + 1):
	                train_x, train_y = get_train_data()
	                sess.run(train_op, feed_dict={x: train_x, y: train_y})
	
	                # 每一万次画图.
	                if i % 10000 == 0:
	                    times = int(i / 10000)
	                    test_x_ndarray = np.arange(0, 2 * np.pi, 0.01)
	                    test_y_ndarray = np.zeros([len(test_x_ndarray)])
	                    ind = 0
	                    for test_x in test_x_ndarray:
	                        test_y = sess.run(net_out, feed_dict={x: test_x, y: 1})
	                        np.put(test_y_ndarray, ind, test_y)
	                        ind += 1
	                    # 先绘制标准的sin函数的曲线，
	                    # 再用虚线绘制我们计算出来模拟sin函数的曲线
	                    draw_correct_line()
	                    pylab.plot(test_x_ndarray, test_y_ndarray,
	                               '--', label=str(times) + ' times')
	                    pylab.show()
	            print("end.")
	
	if __name__ == "__main__":
	        train()
	
	```