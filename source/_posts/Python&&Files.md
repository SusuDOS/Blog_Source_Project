---
title: Python&&Files
date: 2018-12-17 22:15:58
categories: Python基础
tags: [Python&&Files, Python3文本文件复制]
---

# Python的File操作

>1. 在Python下，对file文件的操作是通过创建一个文件对象，然后调用该文件对象的三个方法实现的。

>2. `open` 函数负责打开文件，并且返回文件对象。

>3. read/write/close 三个方法都需要通过 `文件对象` 来调用,即可实现对文件的所有操作。

## File文件对象的创建

- `open()` 函数常用形式是接收两个参数：文件名(file)和模式(mode)。

```python
open(file, mode='r')
```

- `open()` 函数完整的语法格式为：

```python
open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
```

- 参数说明:

	- file: 必需，文件路径,相对或者绝对路径。
	
	- mode: 可选，文件打开模式。
	
	- buffering: 设置缓冲。
	
	- encoding: 一般使用utf-8。
	
	- errors: 报错级别。
	
	- newline: 区分换行符。
	
	- closefd: 传入的file参数类型。
	
	- opener:

- Mode常用参数

	![](https://i.imgur.com/iuTBMcV.png)

<font color=blue>提示，该网页有具体的参数以及使用说明，可以作为一个文档的查询工具，但是就学习而言过多，容易遗忘，所以只是挑选部分进行学习！</font>

网页如下：[http://www.runoob.com/python3/python3-file-methods.html](http://www.runoob.com/python3/python3-file-methods.html "http://www.runoob.com/python3/python3-file-methods.html")

## File文件的读取
### File文件的读取

<font color=blue>文件的读取，是从头开始，指针不断的滑动，所以一旦将文件读到底后，再读取是不会获取到任何的文件内容的，这点需要明确！</font>

- File文件的普通读取-一次性读取完整。
```python
# 1. 相对路径读取.
# file = open("../this_test/00.txt")

# 2. 绝对路径读取.
file = open("C:/Users/Administrator/Desktop/00.txt")

# 获取文件内容--一次性读取完整.
text = file.read()
print(text)

# 关闭文件对象.
file.close()
```
- File文件的按行读取

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
# 按行读取文件是会读取换行符号的，比如/n.

fo = open("C:/Users/Administrator/Desktop/00.txt", "r+")
print ("文件名为: ", fo.name)
count = 1
while True:
    line = fo.readline()
    if not line:
        break
    print("读取第%d行: %s" % (count, line), end="") # 若无end=""会显得很稀疏。
    count += 1
# 关闭文件对象.
fo.close()

"""显示结果为：""""
C:\Users\Administrator\venv\Scripts\python.exe I:/Own_Python_DeepLearning/file_test.py
文件名为:  C:/Users/Administrator/Desktop/00.txt
读取第1行: 先看一下英语四六级的考试流程 
读取第2行: 8：50---9：00试音时间 
读取第3行: 9：00---9：10播放考场指令，发放作文考卷 
读取第4行: 9：10取下耳机，开始作文考试 
读取第5行: 9：35发放含有快速阅读的试题册(9：40才允许开始做) 
读取第6行: 9：40---9：55做快速阅读 
读取第7行: 9：55---10：00收答题卡一(即作文和快速阅读) 
读取第8行: 9：55---10：00重新戴上耳机，试音寻台，准备听力考试 
读取第9行: 10：00开始听力考试，电台开始放音 
读取第10行: 听力结束后完成剩余考项。 
读取第11行: 11：20全部考试结束。 
读取第12行: 9：55---10：00有五分钟的试音巡台，也就是给你的看题时间
Process finished with exit code 0
```
<font color=blue>window下的ANSI文本内容：</font>

![](https://i.imgur.com/mdpxKqU.png)

<font color=red>若文本格式**utf-8**，Unicode则报错：</font>

### file文件的复制

#### 若要实现对文本文件的复制，分具体情况，有以下可行的执行方法.
- 方法一：一次性读取所有的文件内容进行文本文件的复制.

```python
# 1. 创建文件对象.
file_read = open("000.txt")
file_write = open("000[副本].txt", "w")

# 2. 读取文件内容 写入文件内容.
text = file_read.read()
file_write.write(text)

# 3. 关闭文件对象.
file_read.close()
file_write.close()
```

- 方法二：考虑到若是文本文件其他无比可以一行一行的复制，不过相对于Java的StringBuffered效率明显不足.

```python
# 1. 创建文件对象.
file_read = open("000.txt")
file_write = open("000[副本].txt", "w")

# 2. 读取并写入文件.
while True:   
    text = file_read.readline()
    # 若text为空，非 非真=true，则中断。
    if not text:
        break	
    file_write.write(text)

# 3. 关闭文件对象.
file_read.close()
file_write.close()
```

