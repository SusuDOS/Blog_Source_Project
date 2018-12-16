---
title: Python面向对象编程
date: 2018-12-16 21:30:08
categories: Python基础
tags: [python3&&面向对象, 面向对象bianc]
---

## 面向对象编程 && Python3
* Python面向对象编程区别于Java面向对象编程的小细节。

* python3面向对象编程的具体资料可以通过如下链接查看，并且该文档的质量相当较高。([http://www.runoob.com/python3/python3-class.html](http://www.runoob.com/python3/python3-class.html "http://www.runoob.com/python3/python3-class.html"))

* 若是有一天忘记了，可以回顾查看一下代码示例帮助回顾！

### 成员变量 && 成员方法

- 标准的class类定义，成员变量，成员方法定义且使用如下所示：
```python
class Animal:
    def __init__(self, name, age):
        """"成员变量定义"""
        self.animal_name = name
        self.animal_age = age

    def to_string(self):
        """"成员方法定义"""
        print("动物的名字叫做：%s, 动物的年龄为：%d"
              % (self.animal_name,self.animal_age))


animal = Animal("Tom", 5)

"""以类似于java的toString方法进行比较。"""
animal.to_string()
```
**运行结果为**：`动物的名字叫做：Tom, 动物的年龄为：5`。

* 很显然由于python不需要对变量进行数据类型声明就可以使用的特性，完全可以在外部添加成员变量，或者叫做为类进行属性的添加，比如：
```python
animal = Animal("John", 3)
animal.address = "北京"

"""以类似于java的toString方法进行比较。"""
print("动物的住址在：%s, " % animal.address,end="")
animal.to_string()
```
**运行结果为**： `动物的住址在：北京, 动物的名字叫做：John, 动物的年龄为：3`。

### 权限修饰符 && 伪权限修饰
* python也有对变量的权限修饰符，但是明显与Java的不一样，Java可以通过反射强行使用私有的变量以及方法，但是python的完全不需要那么复杂，因为它只是对私有的变量以及变量名称多加了一些东西，我们可以利用其追加变化后的名称进行数据的调用。

```python
class Animal:
    def __init__(self, name, age):
        """"成员变量定义"""
        self.__animal_name = name # 使用两根连续的下划线，成员变量私有化.
        self.animal_age = age

    def to_string(self):
        """"成员方法定义"""
        print("动物的名字叫做：%s, 动物的年龄为：%d"
              % (self.__animal_name,self.animal_age))

    def __to_private_string(self): # 使用两根连续的下划线，成员方法私有化.
        """"成员方法定义"""
        print("我是私有的方法！","动物的名字叫做：%s, 动物的年龄为：%d"
              % (self.__animal_name,self.animal_age))


animal = Animal("John", 3)

"""以类似于java的toString方法进行比较。"""
# 显然与private修饰的成员变量一样，可以通过类中的非私有方法在外部调用.
# 虽然private修饰的私有成员变量在外部亦可以通过反射强行使用，但是一般情况下是不可以的。
animal.to_string()
print("--------this-is-dividing-line!---------")
print("私有变量--动物名称：%s" % animal._Animal__animal_name)
# 调用私有的成员方法，本质上是伪私有。
# 也就是说python对方法名进行了出来，可以调用处理后的方法名称进行调用私有方法名称.
animal._Animal__to_private_string() # 对象后追加_Animal接私有方法名称.
```
* **运行结果为：**
```python
动物的名字叫做：John, 动物的年龄为：3
--------this-is-dividing-line!---------
私有变量--动物名称：John
我是私有的方法！ 动物的名字叫做：John, 动物的年龄为：3
```
### "静态变量" && 静态方法

* python是没有Java的类似的静态变量的；

* 但是有一个类变量，类似于Java的静态变量；
 
* 但是有有些区别，区别在于Java的可以修改影响到该类的此静态变量，但是python的并不会.

```python
class Animal:
    count = 0

    def __init__(self, name, age):
        """"成员变量定义"""
        self.animal_name = name
        self.animal_age = age
        Animal.count += 1

    def to_string(self):
        """"成员方法定义"""
        print("动物的名字叫做：%s, 动物的年龄为：%d"
              % (self.__animal_name,self.animal_age))


Cat = Animal("Tom", 5)
Dog = Animal("John", 3)
print("测试Dog.count数值：% d." % Dog.count)
print("测试Cat.count数值：% d." % Cat.count)
Dog.count = 10
print("重新测试Cat.count数值：% d." % Cat.count)
print("重新测试Dog.count数值：% d." % Dog.count)

# 查看类变量的值，结果为2，显然与Java的不一样。
print("Animal.count：%d." % Animal.count)
```
**运行结果为：**

```python
测试Dog.count数值： 2.
测试Cat.count数值： 2.
重新测试Cat.count数值： 2.
重新测试Dog.count数值： 10.
Animal.count：2.
```

### 构造方法 && 内部方法

python提供一些内部的方法，比如__init__; __str__; __del__等。

![](https://i.imgur.com/KC6LG49.png)

在ubuntu操作系统中，terminal交互模式下，可以通过创建一个对应的数据类型，在dir()即可查看具体的内置函数。

比如： `list_abc = []` , `dir(list_abc)`！即可...

* ` __str__ `方法：

```pyhton
class Animal:
    def __init__(self, name, age):
        """"成员变量定义"""
        self.animal_name = name
        self.animal_age = age

    def __str__(self):
        """"成员方法定义"""
        return("动物的名字叫做：%s, 动物的年龄为：%d"
              % (self.animal_name,self.animal_age))


Cat = Animal("Tom", 5)
print(Cat)
```
<font color = red> **运行结果为：**</font> `动物的名字叫做：Tom, 动物的年龄为：5` !

### 继承 && 多态

* 对比与Java，最大的区别在于可以多继承，其余的多态与Java的完全一致。

```python
class Animal(object):
    def __init__(self, name, age):
        self.animal_name = name
        self.animal_age = age

    def run(self):
        print("奔跑！")

    def sleep(self):
        print("动物睡觉！")


class Creature(object):
    # def __init__(self, name, age):
    #     self.creature_name = name
    #     self.creature_age = age

    def eat(self):
        print("进食！")

    def drink(self):
        print("饮水！")

    def sleep(self):
        print("生物睡觉！")


class Mankind(Animal, Creature):
    def eat(self):
        print("人类吃熟食！")


li_ming = Mankind("LiMing", 18);
# 继承！
li_ming.sleep()
li_ming.run()

# override重写！
li_ming.eat()
```
<font color = red> **运行结果为：**</font>
```python
动物睡觉！
奔跑！
人类吃熟食！
```
* 显然可以多继承继承，继承的顺序按照括号内从左往右的顺序依次查找对应函数，若找到，则执行对应方法，不再继续找下去，若没有则一直下去，直到报错！

<font color = oxfff> **查看方式：**</font>

`print("显示加载顺序:", Mankind.__mro__)`

<font color = oxfff> **结果为：**</font>

```python
显示加载顺序: (<class '__main__.Mankind'>, <class '__main__.Animal'>, <class '__main__.Creature'>, <class 'object'>)
```

## 静态类方法 && 静态方法

* 这个是python3的独有特性：

```python
class Animal():
    count_num = 0

    def __init__(self, name, age):
        """"成员变量定义"""
        self.animal_name = name
        self.animal_age = age
        Animal.count_num += 1

    # 静态方法，与类属性 && 类的实例皆无关.
    @staticmethod
    def run():
        print("奔跑！")

    # 类方法，一般结合类属性，与类的实例无关.
    @classmethod
    def count(cls):
        print ("创建对象次数：%d" % cls.count_num)


Dog = Animal("Dog", 5)
Cat = Animal("Cat", 3)
Animal.run()

# 类变量，适用范围为，类有关系，实例无关系.
print(Animal.count_num)

# 类方法.
Animal.count()
```
<font color = red> **运行结果为：**</font>
```python
奔跑！
2
创建对象次数：2
```