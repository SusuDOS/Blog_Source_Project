---
title: Ubuntu-TensorFlow-GPU
date: 2019-03-14 21:32:56
categories: 深度学习
tags: [Ubuntu-TensorFlow-GPU, DeepLearning]
---

系统说明：
CUDA9.0要求<font color=blue>**GCC版本是5.x或者6.x**</font>，其他版本不可以，需要自己进行配置，通过以下命令才对GCC版本进行修改。

<font color=red>**特别提醒：重启大法好.**</font>

```bash
# method 1:
sudo reboot

# method 2:
sudo shutdown now
```

## GCC的版本核对问题.

### 版本安装：

```bash
# 事先可以先查看版本的问题，ubuntu查看方式为:program_name --version/-v
# 安装对应的gcc版本.
sudo apt-get install gcc-5
sudo apt-get install g++-5
```

### 通过命令替换掉之前的版本
```bash 
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 50
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 50
```

## 安装CUDA® Toolkit 9.0

CUDA9.0下载地址：[https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=deblocal](https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=deblocal "https://developer.nvidia.com/cuda-90-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=deblocal")

注意，一定是要是CUDA9.0而不是CUDA9.1，否则tensorflow运行时会报错-提示框架不支持问题！

<font color=red>提示：因为没有ubuntu18.04版本，所以先使用16.04版本。</font>

### 安装.

- 切换到下载文件目录，执行：

```bash
sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb
sudo apt-key add /var/cuda-repo-9-0-local/7fa2af80.pub
sudo apt-get update
sudo apt-get install cuda
```

- 安装错误提示：

```bash
Reading state information... Done
Some packages could not be installed. This may mean that you have
requested an impossible situation or if you are using the unstable
distribution that some required packages have not yet been created
or been moved out of Incoming.
The following information may help to resolve the situation:
The following packages have unmet dependencies:
 cuda : Depends: cuda-9-0 (>= 9.0.176) but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```
- 解决问题方式：

```bash
# 卸载驱动.
sudo apt purge nvidia*

# 重新安装驱动，若是没有添加库，提示无法找到包.
sudo apt install nvidia-390

# 则需要执行以下两条再继续执行.
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt install nvidia-390

# 由于源服务器国外，所以存在可能包传送丢失.
sudo apt-get update --fix-missing

```

此时正常情况是可以安装成功的.

### 配置.

- 在CUDA完成安装之后，还需要添加环境变量，打开终端，输入下面的命令：

```bash
export PATH=/usr/local/cuda-9.0/bin${PATH:+:${PATH}}

# if x64 system.
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}

# else x86 system.
export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```


- 配置文件位置
```bash
# 打开文件.
sudo vi ~/.bashrc
# 文件的末尾添加如下：

exportPATH=/usr/local/cuda-9.0/bin${PATH:+:${PATH}} 
exportLD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```
- 手动配置的需要通过该方式刷新.

```bash
# 刷新.
source ~/.bashrc

# 刷新.
sudo reboot
```
上述过程完成了整个的CUDA9.0的安装.

## 安装cuDNN v7.0.

**cuDNN v7.0.**下载地址：[https://developer.nvidia.com/cudnn](https://developer.nvidia.com/cudnn "https://developer.nvidia.com/cudnn")

- **cuDNN如图：**

![](https://i.imgur.com/cXUxmFy.png)

- 第一种下载库的安装方式为：

```bash

# 文件为cudnn-9.0-linux-x64-v7.tgz

# 解压
tar zxvf cudnn-9.0-linux-x64-v7.tgz

# home的文件目录结构.
gmxy@PC-GMXY:~$ tree
.
├── 00.txt
├── cuda
│   ├── include
│   │   └── cudnn.h
│   ├── lib64
│   │   ├── libcudnn.so -> libcudnn.so.7
│   │   ├── libcudnn.so.7 -> libcudnn.so.7.0.5
│   │   ├── libcudnn.so.7.0.5
│   │   └── libcudnn_static.a
│   └── NVIDIA_SLA_cuDNN_Support.txt
├── Desktop
├── Documents
├── Downloads
│   ├── cuda-repo-ubuntu1704-9-0-176-local-patch-4_1.0-1_amd64.deb
│   ├── cuda-repo-ubuntu1704-9-0-local_9.0.176-1_amd64.deb
│   ├── cuda-repo-ubuntu1704-9-0-local-cublas-performance-update_1.0-1_amd64.deb
│   ├── cuda-repo-ubuntu1704-9-0-local-cublas-performance-update-2_1.0-1_amd64.deb
│   ├── cuda-repo-ubuntu1704-9-0-local-cublas-performance-update-3_1.0-1_amd64.deb
│   └── cudnn-9.0-linux-x64-v7.tgz
├── examples.desktop
├── Music
├── Pictures
├── Public
├── Templates
└── Videos

11 directories, 14 files
gmxy@PC-GMXY:~$ 

```
图片：![](https://i.imgur.com/7UAsOxa.png)

```bash
# 安装方式为：
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```
- 后一种打包好的库的安装方式为：

```bash
# 切换到下载目录，如：Downloads，在这里打开终端，之后输入以下命令：

cd ~/Downloads
sudo dpkg -i libcudnn7_7.0.5.15-1+cuda9.0_amd64.deb
```
注意，命令能会由于cudnn版本的细微差异而不同，直接输入前面几个字母再使用tab键补全即可。之后等待完成cuDNN的安装...

## 安装Tensorflow-GPU版本
在这里，我们使用Google官方推荐的安装方式：Virtualenv，创建一个虚拟Python开发环境。

### 安装pip和Virtualenv

```bash
# 打开终端执行-python2：
sudo apt-get install python-pip python-dev python-virtualenv
virtualenv --system-site-packages ~/tensorflow

# or 打开终端执行-python3：
sudo apt-get install python3-pip python3-dev python-virtualenv
virtualenv --system-site-packages -p python3 ~/tensorflow
```
### 此时ubuntu的home目录下产生一个tensorflow文件夹.
- 启用创建的虚拟环境

```bash
# bash, sh, ksh, zsh.
source ~/tensorflow/bin/activate
```
```bash
#  csh,tcsh.
source ~/tensorflow/bin/activate.csh
```
一般ubuntu使用的是sh，所以使用第一个执行环境。
提示：若是使用python2.7，在运行中提示python2.7的pip不再支持更新，但是还是可以使用的，python3不受前面一句话的影响。

```bash
# 更新pip命令行.
easy_install -U pip
```

- 安装TensorFlow-gpu环境

使用默认位置安装，没有科学上网可能会失败.

```bash
# Python2
pip install --upgrade tensorflow-gpu

# python3
pip3 install --upgrade tensorflow-gpu
```
使用国内镜像安装.
```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple tensorflow-gpu
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple tensorflow-gpu
```

- 使用环境激活

```bash
# activate.
source ~/tensorflow/bin/activate

#deactivate.
deactivate
```

## 总结

可能存在的问题：

- 在安装TensorFlow时候未使用指定的框架，会提示框架的不匹配无法使用等问题...

- ubuntu使用的使用在`import tensorflow as tf`提示无法找到tensorflow模块，可能是安装的TensorFlow的python环境不是我们使用的python环境问题...

- 导入模块正常，但是会缺少一些必要的组件，比如提示scipy模块无法导入的问题,解决方法。

```bash

# 手动安装scipy...
pip3 install scipy

# 速度过慢可以将链接的地址复制出来使用迅雷下载，再利用SecureCRT的FTP文件传输过去即可.再执行如下....

pip3 install *****.deb

```