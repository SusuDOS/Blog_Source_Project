---
title: Nvidia-docker2 for ubuntu18.04初试！
date: 2019-03-14 21:32:56
categories: Nvidia-docker2
tags: [Ubuntu-TensorFlow-GPU, DeepLearning]
---
# NVIDIA-Docker2深度-人工智能-tensorflow环境搭建
这篇文章是作为我在ubuntu18.04LTS上搭建我的深度环境并且运行的最悲催的血泪史，主要是记录我的环境搭建的问题，以便将来有一天电脑再over的时候作为一种快速回血的方式，同时以此纪念我的耗费三天时间的无力感。

我想我需要把我觉得最重要的一件事情写在前面，<font color=red>请务必学会利用VPS搭建自己的科学上网服务器，第二件事请学会在ubuntu上将利用ss+polipo实现电脑的科学上网</font><font color=blue>(**必须是全局模式的！**，本文没有使用ss+polipo...</font>)

## nvidia-docker2的搭建

正如TensorFlow官方文档所说：[https://tensorflow.google.cn/install/docker](https://tensorflow.google.cn/install/docker)

使用TensorFlow最方便的事情就是利用docker去实现了，因为自从nvidia-docker2开始不再需要自己安装CUDA，这一个是非常重要的事情。至于功能方便的增加修改，daemon的各种优化，不再使用等问题对我等只使用的人而言并没有什么太大的理解价值。

最常用的框架是TensorFlow，利用NVIDIA-docker2再pull的镜像去运行我们所需要的脚本或者是py程序可能是最合适的方式。

### nvidia-docker2的前提条件

- 在本地主机上安装 `Docker`。

- 安装NVIDIA的显卡驱动。
- 安装cuDNN(可以不用安装，但是我认为安装也是可以的！
- 其他环境问题：[https://blog.csdn.net/CS_GaoMing/article/details/88561764](https://blog.csdn.net/CS_GaoMing/article/details/88561764)

#### `Docker` 安装步骤

可以借鉴该文章：[https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04#step-1-%E2%80%94-installing-docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04#step-1-%E2%80%94-installing-docker "https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04#step-1-%E2%80%94-installing-docker")

<font color=green>友情提示：其实与官方的描述大同小异，只不过该文章将具体的实现步骤简化了。</font>

官方Ubuntu存储库中提供的Docker安装包可能不是最新版本。为了确保我们获得最新版本，我们将从官方Docker存储库安装Docker。为此，我们将添加一个新的包源，从Docker添加GPG密钥以确保下载有效，然后安装该包。

- step1 首先，更新现有的包列表：

	```bash
	sudo apt update
	
	# 或者是这样亦可...
	sudo apt-get update
	```
- step2 接下来，安装一些允许apt通过HTTPS使用包的必备软件包：

	```bash
	sudo apt install apt-transport-https ca-certificates curl software-properties-common
	```
- step3 然后将官方Docker存储库的GPG密钥添加到您的系统：

	```bash
	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
	```
- step4 将Docker存储库添加到APT源：

	```bash
	sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
	```
- step5 接下来，使用新添加的repo中的Docker包更新包数据库：

	```bash
	sudo apt update
	```
- step6 确保您要从Docker repo而不是默认的Ubuntu repo安装：

	```bash
	apt-cache policy docker-ce
	```
	虽然Docker的版本号可能不同，但您可能会看到这样的输出：

	```bash
	apt-cache policydocker-ce output
	docker-ce:
	  Installed: (none)
	  Candidate: 18.03.1~ce~3-0~ubuntu
	  Version table:
	     18.03.1~ce~3-0~ubuntu 500
	        500 https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
	```

	请注意，docker-ce未安装，但安装的候选者来自Ubuntu 18.04（bionic）的Docker存储库。

- step7 最后，安装Docker：

	```
	sudo apt install docker-ce
	```
- step8 现在应该安装好了Docker，守护进程启动，并启用进程启动进程。检查它是否正在运行：

	```bash
	sudo systemctl status docker
	```
	输出应类似于以下内容，表明该服务处于活动状态并正在运行：
	```bash
	Output
	● docker.service - Docker Application Container Engine
	   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
	   Active: active (running) since Thu 2018-07-05 15:08:39 UTC; 2min 55s ago
	     Docs: https://docs.docker.com
	 Main PID: 10096 (dockerd)
	    Tasks: 16
	   CGroup: /system.slice/docker.service
	           ├─10096 /usr/bin/dockerd -H fd://
	           └─10113 docker-containerd --config /var/run/docker/containerd/containerd.toml
	```

##### 在没有Sudo的情况下执行Docker命令----（可选）

默认情况下，该docker命令只能由root用户或docker组中的用户运行，该用户在Docker的安装过程中自动创建。如果您尝试运行该docker命令而不使用sudo或不在docker组中作为前缀，您将获得如下输出：

```bash
Output
docker: Cannot connect to the Docker daemon. Is the docker daemon running on this host?.
See 'docker run --help'.
```
如果要sudo在运行docker命令时避免键入，请将用户名添加到docker组中：

```bash
sudo usermod -aG docker ${USER}
```
要应用新的组成员身份，请注销服务器并重新登录，或键入以下内容：

```bash
su - ${USER}
```
系统将提示您输入用户密码以继续。

通过键入以下内容确认您的用户现已添加到docker组：
```bash
id -nG
Output
sammy sudo docker
```
如果您需要将用户添加到docker您未登录的组中，请明确声明该用户名：

```bash
sudo usermod -aG docker username
```
<font color=green>如果您选择不这样做，请在前面添加命令sudo或者切换到su</font>。

#### Nvidia的安装方式.

至于nvidia显卡驱动的安装方式一般上而言就是只有这三种。

嗯，这里也甩一个参看网址：[https://linuxconfig.org/how-to-install-the-nvidia-drivers-on-ubuntu-18-04-bionic-beaver-linux](https://linuxconfig.org/how-to-install-the-nvidia-drivers-on-ubuntu-18-04-bionic-beaver-linux "https://linuxconfig.org/how-to-install-the-nvidia-drivers-on-ubuntu-18-04-bionic-beaver-linux")

虽然也英文的，但是在墙内还是可以看的，看不懂的请试用有道2.0网页插件，或者是chrome浏览器直接右键翻译成简体中文即可。
- method1

	```bash
	ubuntu-drivers devices
	```
	会显示类似这样的界面.

	```bash
	pc-gmxy@pc-pc-gmxy:~$ ubuntu-drivers devices
	== /sys/devices/pci0000:00/0000:00:02.0/0000:01:00.0 ==
	modalias : pci:v000010DEd00001380sv00001462sd00003102bc03sc00i00
	vendor   : NVIDIA Corporation
	model    : GM107 [GeForce GTX 750 Ti]
	driver   : nvidia-driver-415 - third-party free
	driver   : nvidia-driver-410 - third-party free
	driver   : nvidia-driver-418 - third-party free recommended
	driver   : nvidia-340 - distro non-free
	driver   : nvidia-driver-390 - distro non-free
	driver   : nvidia-driver-396 - third-party free
	driver   : xserver-xorg-video-nouveau - distro free builtin
	```

	然后自动安装或者是手动.
	
	```bash
	# 自动安装方式.
	sudo ubuntu-drivers autoinstall
	# 手动安装方式.
	sudo apt install nvidia-driver-415
	```

	<font color=red>请重启电脑，重启电脑最重要，有什么事情请重启再说！</font>


- method2

	```bash
	sudo add-apt-repository ppa:graphics-drivers/ppa
	sudo apt update
	```
	然后跳到method1，over！
- method3

```bash
# 查看显卡.
lshw -numeric -C display
# or这样...
$ lspci -vnn | grep VGA

# 浏览器取官方网站下载显卡驱动的bin文件。

#切换到下载文件夹
cd Down+Tab
#or在下载文件夹下，右键打开终端，或者Ctrl+Alt+T打开终端，类似于这样的文件。
NVIDIA-Linux-x86_64-410.73.bin

# 编译bin文件，前提条件！
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install build-essential libc6:i386

#再然后我都不想看了，麻烦，而且编译源码时间花费很感人，于是....告辞！
```
<font color=red>请重启电脑，请重启电脑，请重启电脑，有什么事情请重启再说！</font>
### nvidia-docker2的安装

重点来了，基本上前面的安装若是无误基本上这一步就是可以正常安装的，可是这一步巨坑，比天坑还坑。

目前存在的我遇到的问题有三大类：

第一种就是：某些文件下载失败，被忽略或者过时，解决办法，在github的issue上给出了n多种方法，我试过了许多种给解决了，就是多添加几种镜像源。

第二种就是：版本的依赖性问题，解决方式有n多中，什么保证最新版安装啊，嗯哼，告辞！根本解决不了问题，方法只有一个就是按照前面的步骤，务必保证步骤中不出错至于给出的用老版本的问题，你看一下TensorFlow+cuda+驱动的版本对应问题吧，若是你掉进这个坑一天之内别想起来你了。

第三种就是：***.d/list*文件第一行错误，无法读取，这个github的issue也有，目前已关闭，解决方式是删除了，重新`sudo apt-get update`这个也是天坑---根本解决不了这个问题的。

<font color=red>核心：请试用ss+polipo实现全局 科学上网 ，同时更改源地址，网易的，阿里的都可以，最重要的是将索引的ip都添加进去，界面方式实现的话只有一个ip。</font>

```bash
# 正常情况.
sudo apt-get install nvidia-docker2
sudo pkill -SIGHUP dockerd
```

正常的情况下实现很简单的，但是重点是基本这一步的问题都是找不到与nvidia-docker2文件子类的。

解决方法，不能科学上网的话，嗯，到此基本上就死了。问题就是源库-镜像源库找不到对应的文件，所以失败....

<font color = red>多次尝试还不成功，请务必使用：'sudo apt-get update --fix-missing'，保证中间的环节不出问题亦可安装！</font>

## nvidia-docker2的镜像拉取

- 实现测试nvidia-docker2时候安装成功.

```bash
docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi

# 其实我认为完全没有必要，要相信自己！
nvidia-smi
docker run --runtime=nvidia --rm hello-world
```
因为没有镜像文件，所以会下载，cuda镜像，这个大概会下载1h+...

显示的是显卡状况,只要弹出unable/unlocal，然后会下载一串文件就是正常安装的。

```bash
# 只是检查驱动可以这样...
nvidia-smi

# 每秒刷新10次，可以这样！
watch -n 0.1 -d nvidia-smi
```

- 拉取镜像

```bash
#拉取之前可以查看有哪些镜像，或者搜索一下有哪些镜像。
# 拉取TensorFlow-gpu镜像
sudo docker run -it tensorflow/tensorflow:latest-gpu 
```
- 搜索镜像

```bash
sudo docker search tensorflow-gpu
```
## nvidia-docker2的镜像/TensorFlow环境的运行.
- 比如，我写好了一个py程序，我需要 该程序 在我的tensorflow-gpu-py3的镜像中运行.
```bash
# 首先我需要加载镜像.
docker run tensorflow-gpu-py3 #显然使用这种格式运行是不可行的，因为没有制定好nvidia-docker2的运行环境，除非已经设置好配置文件.

# 使用交互方式方式:-it;后台方式：-d，-p指定端口.
docker run --runtime=nvidia -it -p 8888:8888 tensorflow/tensorflow:tensorflow-gpu-py3

# 若不小心关闭了，如何进入容器.
sudo docker ps -a 

# 找到容器的一串id，如：62fflkee1e1，键入如下，即可进入. 
sudo docker exec -it 62fflkee1e1 /bin/bash 

# 复制文件到容器之中：docker cp 源路径 目标路径 
docker cp ~/MyTest.py mycontainerID:/opt/MyWorK/

# 调换源路径/目标路径，效果可逆.

# 容器id=d9b10f2f636，运行/停止/删除容器.
docker start/stop/rm d9b10f2f636
```
- 进入容器之后，显示如下：
```bash
root@d9b10f2f636:
# 运行我们的Mytest 文件.
python3 /opt/MyWorK/MyTest.py

# 其他直接在加载镜像时候即可运行不列举.
```
