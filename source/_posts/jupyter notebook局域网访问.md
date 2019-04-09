---
title: ubuntu下的virtual环境配置jupyter notebook局域网访问
date: 2019-04-01 17:49:24
categories: jupyter notebook
tags: [jupyter notebook, tensorflow环境局域网访问]
---


## ubuntu下的virtual环境配置jupyter notebook局域网访问

深度学习中，若是局域网内有一台运行ubuntu的主机是一件非常方便的事，很多事情就会变得容易处理。比如，练习ubuntu命令，学习配置深度学习环境，运行jupyter服务。

由于我们要保证系统的稳定性，也就是说我们的主机是存在出故障的可能性的，所以我建议的是针对不同的服务，将不同的环境单独建立起来存在虚拟环境中，使用的时候，激活需要使用的环境，在该环境下运行对应的命令行即可。

PS:<font color = red>推荐使用python3.*版本！</font>

- ubuntu创建自己的虚拟环境.

```bash
# 安装创建虚拟环境所需要等待对应的安装包和创建虚拟环境名称.
sudo apt-get install python3-pip python3-dev python-virtualenv

# 名称为tensorflow，位置为当前用户的home目录下.
virtualenv --system-site-packages -p python3 ~/tensorflow
```

- 激活环境&安装对应安装包

```bash
source ~/tensorflow/bin/activate

# 更新pip命令行，其实这个是苹果系统的更新命令，可能源于linux吧，使用并没有任何问题.
easy_install -U pip

# 安装jupyter，包名有好几个，使用不同的包名安装并没有什么问题。
# 使用清华镜像目前可能是最好的选择，理由是速度相对于阿里云，网易云，速度会差一点，但是稳定，可能是习惯使然吧.
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple jupyter ipython vim

# 生成配置文件，默认生成文件的位置为:~/.jupyter/jupyter_notebook_config.py
jupyter notebook --generate-config

# 配置文件信息.
ipython

In [1]: from notebook.auth import passwd
In [2]: passwd()

# 以上由ipython对话式，生成密文.
Enter password: 
Verify password: 
Out[2]: 'sha1:5462a9592a3d:e502996ab12a5564638f588bd1bd661fcf8b9ad7'

# 在~/.jupyter/jupyter_notebook_config.py末尾添加.
#c.ConnectionFileMixin.ip = '0.0.0.0'#若有问题请添加改行上去...
c.NotebookApp.ip = '0.0.0.0'
c.NotebookApp.open_browser = False
c.NotebookApp.password = 'sha1:5462a9592a3d:e502996ab12a5564638f588bd1bd661fcf8b9ad7'
c.NotebookApp.port = 12345
```

信息如图：
![演示样例0002.PNG](https://i.loli.net/2019/04/01/5ca1d83b21080.png)

- 运行`jupyter notebook`

```bash
# 后台运行jupyter，且将重定向文件定位为，当前用户home目录下的log目录下的jupyter.log文件中.
nohup jupyter notebook >~/log/jupyter.log 2>&1&
```

默认是没有该文件的，可以vi ~/log/jupyter.log，会生成对应文件目录+文件.或者：

```bash
mkdir ~/log/
touch jupyter.log

# 再后台运行.
nohup jupyter notebook >~/log/jupyter.log 2>&1&

# or直接运行
jupyter notebook
```

浏览器登录如图：
![338900.PNG](https://i.loli.net/2019/04/01/5ca1d8d9cb2f2.png)
![338901.PNG](https://i.loli.net/2019/04/01/5ca1d8d9eeb1d.png)

- 结束该程序

后台运行一旦关闭该窗口，是无法通过其他terminal窗口查看该进程的，可以通过：

```bash
ps aux | grep jupyter，查看进程pid.

# 干掉该后台进程.
kill -9 pid编号.
```
如图示例：
![演示样例0001.PNG](https://i.loli.net/2019/04/01/5ca1d7f679c52.png)

特别链接：后台运行命令行，[https://www.ibm.com/developerworks/cn/linux/l-cn-nohup/index.html](https://www.ibm.com/developerworks/cn/linux/l-cn-nohup/index.html "https://www.ibm.com/developerworks/cn/linux/l-cn-nohup/index.html")