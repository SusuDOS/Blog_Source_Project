---
title: Python&exception
date: 2018-12-17 19:26:30
categories: Python基础
tags: [PythonException, exception]
---

# exception && package操作

## Python&&Exception

### 异常的完整捕捉格式
```python
# exception完整捕捉逻辑代码！
try:
    num1= int(input("输入一个整数："))
    num2 = int(input("请再输入输入一个整数："))
    result = num1 / num2
    print(result)

# 已知异常的捕捉.
except ValueError:
    print("请输入整数!")
except ZeroDivisionError:
    print("除数不能为0！")

# 未知异常的捕捉.
except Exception as result:
    print("未知异常 %s" % result)

# 无异常则执行.
else:
    print("代码运行正常，无异常产生！")

# 有无异常均正常执行！
finally:
    print("无论是否出现异常，均执行的代码！")

print("*" *35)
```
<font color=red>**运行结果：**</font>

```python
输入一个整数：45
请再输入输入一个整数：0
除数不能为0！
无论是否出现异常，均执行的代码！
***********************************
输入一个整数：12
请再输入输入一个整数：p
请输入整数!
无论是否出现异常，均执行的代码！
***********************************
输入一个整数：45
请再输入输入一个整数：45
1.0
代码运行正常，无异常产生！
无论是否出现异常，均执行的代码！
***********************************
```

<font color=blue>自定义异常：有时候出错的提示可能并不友好，或者是实际的需要自己去定义异常，定义异常及其捕捉如下。</font>

```python
def check_password():
    pwd = input("请输入密码：")
    if len(pwd) >= 8:
        return pwd
    # 密码长度不够，则自定义异常并且抛出.
    print("自定义异常抛出！")
    ex = Exception("异常：密码长度不够！")
    raise ex


try:
    print(check_password())
except Exception as result:
    print(result)

print("*" *35)
```

<font color=red>**运行结果：**</font>

```python
请输入密码：qweeeeeee
qweeeeeee
***********************************
请输入密码：qwee
自定义异常抛出！
异常：密码长度不够！
***********************************
```
### 异常提交到上一层 

* 以下例子虽然举的不恰当，但是也可以看出，可以通过return将其提交到上一层函数.

```python
def input_test():
    return int(input("输入整数："))


def test():
    return input_test()


try:
    print(test()) # 本质上是input_test，但是反应在test函数.
except Exception as result:
    print("未知错误：%s" % result)
```

<font color=red>**运行结果：**</font>

```python
输入整数：PP
未知错误：invalid literal for int() with base 10: 'PP'
```

## Package && Python

<font color = blue>此处重点区分包的导入与模块导入的区别，以及掌握包/模块导入的注意事项，以及推荐的使用导入方式。</font>

### 模块的导入

>1. 模块的概念：
模块是 Python 程序架构的一个核心概念,每一个以扩展名 `py` 结尾的 `Python` 源代码文件都是一个 模块。

>2. 模块的两种导入方式:
```python
# method1.
import module_1, module_2

# method2，推荐此法.
import module_1
import module_2 
```
```python
# python的别名导入,一般是模块名称太长导致的.
import numpy as np
```
注意：模块别名应该符合大驼峰命名法.

### 驼峰命名法

- 小驼峰法
变量一般用小驼峰法标识。驼峰法的意思是：除第一个单词之外，其他单词首字母大写。譬如:
	```java
	int myStudentCount;
	```
- 大驼峰法
相比小驼峰法，大驼峰法（即帕斯卡命名法）把第一个单词的首字母也大写了。
常用于类名，命名空间等，譬如：
	```java
	public class DataBaseUser;
	```

- `from...import` 导入

如果希望从某一个模块中，导入 `部分` 工具，就可以使用 `from ... import` 的方式导入。区别于 `import`，`import` 是把模块中所有工具全部导入，并且通过 `模块名/别名` 访问。

```python
# for example
from random import randint # (as short_name)
rand_number = randint(0, 100)
print(rand_number)
```

- **`from...import *`**
```python
# 导入所在模块的所有工具函数.
from numpy import *
```
<font color = blue>注意:这种方式不推荐使用，因为函数重名/函数被覆盖/运行的不是语气函数 并没有任何的提示。</font>


<font color = red>**任何时刻，都要注意重名的问题！**</font>

```python
# 将所有的部分都写在main函数里面，方面测试使用，但是不希望导包的时候执行该部分.
def main():
    # ...
    pass

# 根据 __name__ 判断是否执行下方代码，true表示执行，否则不执行.
if __name__ == "__main__":
    main()
```

### 包的制作&&发布
>1. 保持的目录结构，__init__文件内容.

```python
from . import test_one
from . import test_two
```

![](https://i.imgur.com/v6YcUA0.png)

>2. setup文件内容

```python
from distutils.core import setup

setup(name="test_main",  # 包名
      version="0.0.1",  # 版本
      description="test_main测试组模块.",  # 描述信息
      long_description="test_main测试组模块.",  # 完整描述信息
      author="test_cc",  # 作者
      author_email="1234567@qq.com",  # 作者邮箱
      url="www.baidu.com",  # 主页
      py_modules=["test_main.test_one",
                  "test_main.test_two"])
```

>3. 构建模块.

```python
python3 setup.py build
```

>4. 生成发布压缩包.
```python
python3 setup.py sdist
```
注意：解释器的版本问题！

### 制作后包的安装

<font color=blue>为保证顺序的完整性且保证分段阅读，此处不再重新开始顺序，继续从第五位置开始。</font>

>5. 安装模块

```python
tar -zxvf test_main-0.0.1.tar.gz 
sudo python3 setup.py install
```

> 6. 卸载模块

直接从安装目录下，把安装模块的目录删除就可以.

```python
cd /usr/local/lib/python3.5/dist-packages/
sudo rm -r test_main*
```

```python
import random

#  查看内置函数.
print(random.__dir__())
# 查看函数模块位置.
print(random.__file__)
```

## `pip` 安装第三方库

`pip` 是一个现代的，通用的 `Python` 包管理工具，提供了对 `Python` 包的查找、下载、安装、卸载等功能。

安装和卸载命令如下：
```python
# 将模块安装到 Python 2.x 环境.
sudo pip install pygame
sudo pip uninstall pygame

# 将模块安装到 Python 3.x 环境.
sudo pip3 install pygame
sudo pip3 uninstall pygame
```

在 Mac 下安装 iPython

```python
sudo pip install ipython.
```

在 Linux 下安装 iPython
```python
sudo apt install ipython
sudo apt install ipython3
```
