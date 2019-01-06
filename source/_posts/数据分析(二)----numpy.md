---
title: 数据分析(二)----Numpy
date: 2019-01-06 20:27:40
categories: Python-数据分析
tags: [数据分析-Numpy, Numpy]
---

- numpy读取文件函数.
```python
np.loadtxt(fname,dtype=np.float,delimiter=None,skiprows=0,usecols=None,unpack=False)
```
- 各个参数释义.

![](https://i.imgur.com/4lqNPkk.png)


### 矩阵的操作

- 矩阵模块numpy的导入.

```python
import numpy as np
```

***

- 矩阵的创建

```python
t1 = np.arange(10,stype="float=32")
t1 = np.array([x for x in range(10)],dtype="float32")
t1 = np.array(range(10),dtype="float")
```

- 矩阵的形态变化

```python
# 结果需要保存需要新变量接受.
t1.reshape(2,5)
```

- 矩阵的元素类型查看

```python
t1.dtype
```

- 矩阵的元素类型修改

```python
t2 = t1.astype("int")
```

- 矩阵的转置操作为:

```python
# 三种不同的方式.
t1.transpose()
t1.T
t1.swapaxes(0,1)
t1.swapaxes(1,0)
```

- 矩阵的四则运算

	- 行列相同，对应元素进行四则运算.
	- 二维:行列不同 对应的行 or 列必须一样，且为一维数组：`n*1` or `1*n`.
	- 高维： 需要`（.*）m*n` 与 `m*n` 对应格式匹配.


***

- 矩阵的文件读取

```python
file_object=np.loadtxt(fname,dtype=np.float,delimiter=None,skiprows=0,usecols=None,unpack=False)
# eg.
import numpy as np
file_object=np.loadtxt("./NewTemp.csv",delimiter=",",unpack=True)
```

***

- 矩阵的切片
- 行列规则：横着的一排为行:row；竖着的一排为列:col.
- axis的规则：(3,4,5)按照从左往右0轴，1轴，2轴。0轴为块，1轴为行，2轴为列.

```python
# t1为6*7.
t1 = [[ 0  1  2  3  4  5  6]
 [ 7  8  9 10 11 12 13]
 [14 15 16 17 18 19 20]
 [21 22 23 24 25 26 27]
 [28 29 30 31 32 33 34]
 [35 36 37 38 39 40 41]]
```

- 取单行

```python
print(t1[1])
```

- 取连续多行

```python
print(t1[2:5])

print(t1[2:])
```

- 取不连续多行

```python
# 取2，3，5行.
print(t1[[2,3,5]])
```

- 取单列

```python
print(t1[1：])
```

- 取连续多列

```python
# 第三列到最后列...
print(t1[:,2:])
```

- 取不连续多列

```python
# 取第一列，第三列.
print(t1[:,[0,2]])
```

- 取多行多列

```python
# 取的行列交集部分.
# 取第一行到第三行，第二列到第四列值.
print(t1[0:2,1:3])
```

- 取某元素值

```python
# 显示t1第四行第六列的值.
print(t1[3,5])
```

- 取多个元素值

```python
# (0,1)(1,2)(2,2)(3,4),行列分开...
print(t1[[0,1,2,3],[1,2,2,4]])
```

***

- 矩阵赋值-利用bool索引实现

```python
# 
import numpy as np
t1 = np.arange(15).reshape(3,5)
t1[t1>9] = 99
print(t1)

# 将大于10的部分替换为99.
# 结果显示为：
[[ 0  1  2  3  4]
 [ 5  6  7  8  9]
 [99 99 99 99 99]]
```

- 矩阵赋值-利用where实现

```python
import numpy as np
t1 = np.arange(15).reshape(3,5)
t2 = np.where(t1<8,0,6)
print(t2)

# 将小于8的部分替换为，其他部分替换为6.
# 结果显示为：
[[0 0 0 0 0]
 [0 0 0 6 6]
 [6 6 6 6 6]]
```

- 矩阵赋值-利用clip裁剪实现.

<font color=blue>若有元素为nan则该元素不替换。</font>

```python
import numpy as np
t1 = np.arange(15).reshape(3,5)
t2 = t1.clip(8,12)
print(t2)

# 将小于8的替换为8，大于12的替换为12.
# 运行结果为： 
[[ 8  8  8  8  8]
 [ 8  8  8  8  9]
 [10 11 12 12 12]]
```

***

- 矩阵拼接-分割

```python
# 水平拼接.
np.hstack(t1,t2)

# 竖着拼接
np.vstack(t1,t2)
```

- 矩阵的行交换 or 列交换.

```python
# 行交换.
t1[[1,2],:]=t1[[2,1],:]

# 列交换.
t1[:,[1,2]]=t1[:,[2,1]]
```

- 矩阵元素最大值索引
	- 使用函数np.argmax(t,axis=0)

```python
import numpy as np
import random
t1 = np.array([random.random() for i in range(30)]).reshape(5, 6)
print(t1)
tca = np.argmax(t1, axis=0)
tcb = np.argmax(t1, axis=1)
print(tca)
print(tcb)
print(t1.ndim)

# 运行结果为：
[[0.9477623  0.81062099 0.24186812 0.26153176 0.04750822 0.94339204]
 [0.95930415 0.29915089 0.12912976 0.9774711  0.84254131 0.12406201]
 [0.24721583 0.69608407 0.84093561 0.82350435 0.91174074 0.68815985]
 [0.35198547 0.68184997 0.14498233 0.49661146 0.85298706 0.57192801]
 [0.9295951  0.94717111 0.40329606 0.92547673 0.14411297 0.14591956]]
[1 4 2 1 2 0]
[0 3 4 4 1]
2
```

***

- 创建单位矩阵

```python
# 创建5阶单位矩阵
new_temp = np.eye(5)
```

- 创建零矩阵

```python
# 创建零矩阵
new_temp = np.zeros((3,4))
```
- numpy的随机数

![](https://i.imgur.com/fKXr1fR.png)

- numpy的随机数举例操作如：

```python
# 4*5 的[10,20]的随机数组...
# 加入随机种子后，唯一：np.random.seed(10)
import numpy as np
temp = np.random.randint(10,21,(4,5))
```

- 矩阵的复制

	- 矩阵的赋值
	
	```python
	# 该操作等价于标签移动而已，完全关联.
	temp_01 = temp
	```
	- 矩阵的切片
	
	```python
	# 创建新的对象，但是b的数据的改变会影响temp_02的数据变化.
	temp_02 = b[:]
	```
	- 矩阵的复制
	
	```python
	# 两个矩阵是完全独立的，互不干涉.
	new_temp = temp.copy()
	```
- 矩阵赋值Demo

```python
# 范例如下：
import numpy as np


temp = np.random.randint(10,21,(3,4))
print("source_matrix:\n",temp)
print("操作：","*"*10)

temp_01 = temp
temp_02 = temp[:]
temp_new = temp.copy()

temp[2,2] = 100

print("显示结果：",">"*10)
print("source_matix:\n",temp,"\n")

print("temp_01:\n",temp_01,"\n")

print("temp_02:\n",temp_02,"\n")

print("temp_new:\n",temp_new,"\n")

```

结果显示为：

![](https://i.imgur.com/ruT6A1t.png)

****

### 矩阵的四舍五入

- 四舍五入-不带参数-返回结果为整数.

```python
# 一般满足四舍五入原则，特殊点在中间举例的时候的取舍问题.

# 结果为4，靠近偶数的那个取值.
round(3.5)
round(4.5)


# 负数仍然符合该规律,结果为:-2.
round(-1.5)
round(-2.5)
```

- 四舍五入，处理的是浮点数.
<font color = red>本人完全没有找到有效的规律，一脸懵逼，有博文提示：“四舍五入”的前一位，奇数：舍尾，偶数：收尾；本人实测部分正确，如下：</font>

```python
print(round(2.635,2))
print(round(2.645,2))
print(round(2.655,2))
print(round(2.665,2))
print(round(2.675,2))

# 执行结果为：
2.63
2.65
2.65
2.67
2.67
```

但是失效部分如下：

```python
# 我们换一个数字试一下.
print(round(2.525,2))
print(round(2.535,2))
print(round(2.545,2))
print(round(2.555,2))
print(round(2.565,2))
print(round(2.575,2))
print(round(2.585,2))

# 执行结果如下：
2.52
2.54
2.54
2.56
2.56
2.58
2.58
```

神奇的发现，此时上面的 `定则` 不再有效。但是小数点刚好是奇偶搭配，于是认为可能满足此规则，但是：

```python
print(">" * 30)
print(round(1.525,2))
print(round(1.535,2))
print(round(1.545,2))
print(round(1.555,2))
print(round(1.565,2))
print(round(1.575,2))
print(round(1.585,2))
print(round(1.595,2))

print("." * 30)
print(round(2.525,2))
print(round(2.535,2))
print(round(2.545,2))
print(round(2.555,2))
print(round(2.565,2))
print(round(2.575,2))
print(round(2.585,2))
print(round(2.595,2))

print("*"*30)
print(round(2.625,2))
print(round(2.635,2))
print(round(2.645,2))
print(round(2.655,2))
print(round(2.665,2))
print(round(2.675,2))
print(round(2.685,2))
print(round(2.695,2))
```
运行结果为：

![](https://i.imgur.com/AwCzbQY.png)

**<font color = red>结论：貌似并没有想象的关系,显然就效果而言不是可控的。唯一可能的是在存储的底部，如IEEE-754浮点数存储规则，有自己的移位运算规则，但是就转化成的十进制数据而言就是不可控了..</font>**

*** 

### nump注意事项

- np.nan != np.nan
- 判断元素为nan个数.
- nan与任何值计算都为nan.
- 计算矩阵的sum值，均值，中值.
```Python
# 若元素有nan，返回结果为nan.
np.sum(t)

# 计算行和.
np.sum(t,axis=0)

# eg：
import numpy as np 

t = np.random.randint(1,11,(4,5))
print(t)
print(">" * 30,"\n")
print(np.sum(t, axis=0)) # print(t.sum(axis=0))等价的.

# 平均值:mean;中值：median
print(t.sum())

# 运行结果：
[[4 9 3 5 8]
 [5 6 6 2 1]
 [3 2 5 6 5]
 [5 4 5 3 8]]
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>

[17 21 19 16 22]
95

```

```python
# t!=t返回的是bool矩阵.

# 此时返回的是元素为nan的个数.
np.count_nonzero(t!=t)
```

```python
# 统计非零个数.
np.count_nonzero(matrix_name)
``` 

- numpy统计函数

**返回数值规则：**

**<font color=blue>默认返回多维数组的全部的统计结果,如果指定axis则返回一个当前轴上的结果。</font>**

	- 求和：t.sum(axis=None)
	- 均值：t.mean(a,axis=None)  
	- 中值：np.median(t,axis=None) 
	- 最大值：t.max(axis=None) 
	- 最小值：t.min(axis=None)
	- 极值：np.ptp(t,axis=None)
	- 标准差：t.std(axis=None)

### numpy的nan处理

- 将元素为nan的修改为我们想要的数值.
	- 修改为平均值，中值等.
	- 将nan元素置0
	- np.isnan(matrix_fname)

code_picture:

![](https://i.imgur.com/BPfz2ju.png)

```python
import numpy as np 

def adjust_ndarray(matrix):
	for i in range(matrix.shape[1]):
		temp_col = matrix[:,i]
		nan_num = np.count_nonzero(temp_col != temp_col)
		if(nan_num != 0):
			# 新的列获取，跳过为false的元素.
			new_temp = temp_col[temp_col == temp_col]

			# 为nan元素被mean覆盖掉.
			temp_col[np.isnan(temp_col)] =  new_temp.mean()
	return matrix


def zero_matrix(matrix):
	for i in range(matrix.shape[1]):
		temp_col = matrix[:,i]
		temp_col[np.isnan(temp_col)] = 0
		# 置为0以后明显是可以计算平均值的，但是为遍历为0的数值修改为mean不可行，
		# 因为其本身就是有可能是0，因此上述的方法是比较合理的nan->mean的方法.
	return	matrix

if __name__ == '__main__':
	source_matrix = np.arange(35,dtype = "float32").reshape(5,7)
	# 与上面一行等价.
	# source_matrix = np.arange(35).reshape(5,7).astype("float32")
	bak_source_matrix = source_matrix.copy()

	print("source_matrix:\n",source_matrix)
	source_matrix[[2,3],3:] = np.nan

	bak_source_matrix = source_matrix.copy()

	print("nan_matrix:\n", source_matrix)

	adjust_matrix = adjust_ndarray(source_matrix)
	print("adjust_matrix:\n",adjust_matrix)

	print(">" * 30)
	fill_zero = zero_matrix(bak_source_matrix)
	print("fill_zero:\n",fill_zero)


# running results:
C:\Users\Administrator\Desktop>python NewTemp.py
source_matrix:
 [[ 0.  1.  2.  3.  4.  5.  6.]
 [ 7.  8.  9. 10. 11. 12. 13.]
 [14. 15. 16. 17. 18. 19. 20.]
 [21. 22. 23. 24. 25. 26. 27.]
 [28. 29. 30. 31. 32. 33. 34.]]
nan_matrix:
 [[ 0.  1.  2.  3.  4.  5.  6.]
 [ 7.  8.  9. 10. 11. 12. 13.]
 [14. 15. 16. nan nan nan nan]
 [21. 22. 23. nan nan nan nan]
 [28. 29. 30. 31. 32. 33. 34.]]
adjust_matrix:
 [[ 0.        1.        2.        3.        4.        5.        6.      ]
 [ 7.        8.        9.       10.       11.       12.       13.      ]
 [14.       15.       16.       14.666667 15.666667 16.666666 17.666666]
 [21.       22.       23.       14.666667 15.666667 16.666666 17.666666]
 [28.       29.       30.       31.       32.       33.       34.      ]]
>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>
fill_zero:
 [[ 0.  1.  2.  3.  4.  5.  6.]
 [ 7.  8.  9. 10. 11. 12. 13.]
 [14. 15. 16.  0.  0.  0.  0.]
 [21. 22. 23.  0.  0.  0.  0.]
 [28. 29. 30. 31. 32. 33. 34.]]
```