---
title: Linux基本操作命令
date: 2018-12-04 17:20:14
categories: linux基础
tags: [linux基础, linux]
---
# Linux系统下基本命令
由于Linux命令大概有200多个，但是实际上的使用约20来个命令行，对于不是太常用的命令行一般采取的方式是直接在API里面搜索相关的命令行或者是针对某一种操作类型的命令行对操作手册进行查阅即可。

我始终认为一本优秀的API查询手册应该具备有命令行分类和支持命令行检索查询的特点，而一篇优秀的引导书应该是对具体的命令行进行筛选，同时给出操作使用的范例方便理解。

**常用的命令行分类及其使用如下：**
## 基本必备命令行
该部分命令行是最基本的，也是使用最为频繁，不容易忘记的。

首先介绍Linux下的命令行格式一般为：`command [-options] [parameter]`例如：
```python
# 查看当前用户主目录下的DeskTop文件下的文件情况.
ls -alh ~/DeskTop
```
其中的 `选项` 的可以是多个，其中的顺序也是无关紧要的，关于`命令行`中若`[parameter]`存在用户名，用户组等参数，一般都是<font color=red> `组`, `用户`, `文件/文件夹` </font>的参数顺序。
### 常用命令行
![](https://i.imgur.com/cDOsoK1.png)

```python
# 显示当前文件夹下文件及其文件夹信息.
ls -alh #or
ls -alh ~/DeskTop # 显示桌面文件夹下文件及其文件夹的信息.

# 不知道我们当前位置，要知道我们处于哪个账户和文件夹下.
pwd

# 用户主目录，切换桌面文件夹位置.
cd DeskTop 

# 创建一个文件夹TestAa.
mkdir TestAa

# 递归创建一个文件夹TestCc.
mkdir -r TestAa/TestBb/TestCc

# 创建一个文件File.txt.
# 若存在则修改日期，否则创建文件.
touch File.txt 

# 递归创建一个文件File.txt.
# 新建目录不能与已有重名.
mkdir -p TestAa/TestBb/TestCc/Files.txt

# 递归删除当前目录下的TestAa文件夹，不带r只能删除空文件夹or删除文件.
rm -r TestAa

# 删除文件.
rm File.txt.

# 屏幕输入太多，清屏重新开始.
clear
```
- cp实现复制功能

格式： `cp Sourcefile Targetfile`。

```
# -i 覆盖文件前提示!
cp -i File.txt File Next.txt

# -r 若给出的源文件是目录文件，则cp将递归复制该目录下的所有子目录和文件，目标文件必须为一个目录名。
cp -r TextAa TestCopy
cp File.txt File Next.txt
```
- mv实现移动功能

格式：`mv Sourcefile Targetfile` 命令可以用来移动 `文件` 或 `目录` ，也可以给文件或目录重命名。
```
# -i 覆盖文件前提示!
# 文件重命名:File.txt->TestDd.txt.
mv File.txt TestDd.txt
```
### 文件内容查看/内容查看

![](https://i.imgur.com/RKOpCzW.png)
```python
# 查看File.txt文件内容.
cat -n File.txt # 一次性完全显示内容，显示行编号.
cat -b File.txt # 一次性完全显示内容，显示非空行编号.

more File.txt # 显示一屏幕.
```
### more显示操作

```python
# 空格键	显示手册页的下一屏.
# Enter 键	一次滚动手册页的一行.
# b	回滚一屏.
# f	前滚一屏.
# q	退出.
# /word	搜索 word 字符串.
```
### grep
Linux 系统中` grep `命令是一种强大的文本搜索工具。
grep允许对文本文件进行模式查找，所谓模式查找，又被称为正则表达式。
```python
# -n	显示匹配行及行号.
# -v	显示不包含匹配文本的所有行（相当于求反）！
# -i	忽略大小写.
```
- 常用的两种模式查找.
```
^a	行首，搜寻以 a 开头的行!
ke$	行尾，搜寻以 ke 结束的行.
```
```python
# 查找File.txt文件中以f开头的行，并且显示行号.
grep -n ^f File.txt
```
### echo与>重定向的联合使用
```python
# 将hello写入到File.txt，文件不存在则创建.
echo hello > File.txt

# 以追加的形式将Hello World写入到File.txt.
echo "Hello World" >> File.txt
```

### 管道 |
Linux允许将**一个命令**的输出,可以通过管道做为**另一个命令**的输入。
```python
# 查找vi开头的列表中的文件名称.
ls -lha ~ | grep vi

# 以more函数查看ls下的文件情况.
ls -lha ~ | more
```

### 友情提示：
```python
# terminal调小调大，Shift是切换= -> +.
Ctrl + -
Ctrl + Shift +  =
```
*<font size=4 color=red>Tab</font><font size=3 color=blue>可以自动补全命令行关键字or文件or文件夹名字。</font>*

## 系统信息命令

![](https://i.imgur.com/99PP31A.png)
![](https://i.imgur.com/ZkMnc7B.png)
![](https://i.imgur.com/0WJ3ZEc.png)
### 系统命令示例
```python
# 显示所有用户所有进进程的详细情况,不需要加短线.
ps aux

# 查看当前用户的有控制台控制进程.
ps
 
# 强制结束terminal进程.
kill -9 termimal
```
### shutdown关机命令
```
# 重新启动操作系统，其中now表示马上.
shutdown -r now

# 立刻关机.
shutdown now

# 系统在今天的 20:25 会关机.
shutdown 20:25

# 系统10min后自动关机.
$ shutdown +10

# 取消之前指定的关机计划
shutdown -c
```

### 网卡命令
#### ping
```python
# 127.0.0.1 被称为本地回环/环回地址，一般用来测试本机网卡是否正常。
# 本质上其实是本机向保留地址127，发送一个广播，广播所有主机-包括主机，若无响应则网卡工作异常，否则是网卡运行正常。
ping 127.0.0.1

# ping网络连接，判断是否是由DNS异常问题导致无法使用网络。
ping www.baidu.com
```
#### ifconfig
* ifconfig 可以查看／配置计算机当前的网卡配置信息.
```python
# 查看网卡配置信息.
ifconfig

# 查看网卡对应的 IP 地址
ifconfig | grep inet

# 结合ping查看到路由器的连接问题.
ifconfig
ping 网关ip.
```

## 远程文件管理

使用ssh工具实现对本地文件到liunx文件的复制下载工作,命令行格式基本一致。

常用工具：Putty，XShell，SecureCRT...
* Putty [http://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
* XShell [http://xshellcn.com](http://xshellcn.com)

### SSH

在 `Linux` 中 `SSH` 是 `非常常用` 的工具，通过SSH客户端 我们可以连接到运行了SSH服务器的远程机器上,如测试学习使用的Linux服务器。

SSH客户端是一种使用 `Secure Shell` 协议连接到远程计算机的软件程序，SSH 是目前较可靠，专为远程登录会话和其他网络服务 提供安全性的协议，传输的数据可以是经过压缩的，所以可以加快传输的速度。

特点：由其名字 `Secure Shell` (SSH)得知，主要是命令行的远程操作和传输，对于文件的传输存在局限性。

SSH服务器的默认端口号是22，如果是默认端口号，在连接的时候，可以省略。

![](https://i.imgur.com/ePQPhRy.png)

#### SSH 客户端的简单使用
格式：`ssh [-p port] user@remoteip`.

```python
# user 是在远程机器上的用户名，如果不指定的话默认为当前用户
# remote 是远程机器的地址，可以是 IP／域名，或者是 后面会提到的别名
# port 是 SSH Server 监听的端口，如果不指定，就为默认值22.
```
```python
ssh -p 22 GaoMing@192.168.111.119
```
#### SSH的高级配置
- 免密码登录
- 配置别名

**提示**：有关 `SSH` 配置信息都保存在用户主(home)目录下的 .ssh 目录下.

- 免密码登录步骤
  - 配置公钥

    执行 ssh-keygen 即可生成<font color=red>SSH</font>钥匙，一路回车即可。

  - 上传公钥到服务器

    执行 ssh-copy-id -p port Gaoming@192.168.111.119，可以让远程服务器记住我们的公钥。


- 非对称加密算法
```
使用 公钥 加密的数据，需要使用 私钥 解密
使用 私钥 加密的数据，需要使用 公钥 解密
```
- 配置别名步骤

每次都输入 ssh -p port user@remote，时间久了会觉得很麻烦，特别是当 user, remote 和 port 都得输入，而且还不好记忆。

而配置别名 可以让我们进一步偷懒，譬如用：ssh mac 来替代上面这么一长串，那么就在 ~/.ssh/config 里面追加以下内容：

Host mac
    HostName ip地址
    User itheima
    Port 22
保存之后，即可用 `ssh mac` 实现远程登录了，`scp` 同样可以使用。

### scp

scp就是 `secure copy`，是一个在 `Linux` 下用来进行 `远程拷贝文件` 的命令。

它的地址格式与 `ssh` 基本相同，需要注意的是，在指定端口时用的是大写的 `-P` 而不是小写的。

```python
# 把本地当前目录下的 TestAa.py 文件 复制到 远程 家目录下的 Desktop/TestAa.py
# 注意：`:` 后面的路径如果不是绝对路径，则以用户的家目录作为参照路径.
scp -P port This.py Gaoming@192.168.111.119:Desktop/This.py

# 把远程主目录下的 Desktop/01.py 文件 复制到 本地当前目录下的 01.py
scp -P port Gaoming@192.168.111.119:Desktop/This.py This.py

# 加上-r选项后只能可以传送文件夹，此时不能传输文件。
# 把当前目录下的Test文件夹复制到远程家目录下的Desktop.
scp -r Test Gaoming@192.168.111.119:Desktop/

# 把远程主目录下的 Desktop复制到当前目录下的Test文件夹.
scp -r Gaoming@192.168.111.119:Desktop Test
```
### FileZilla

官方网站：[https://www.filezilla.cn/download/client](https://www.filezilla.cn/download/client).

 `FileZilla` 在传输文件时，使用的是FTP服务 而不是 SSH 服务，因此端口号应该为21，优点是可视化的文件复制粘贴，支持直接拖拉拖拽的复制粘贴操作，这点上有点类似于windows上的远程登录界面后直接拖拽复制文件...

## 用户权限

用户是Linux 系统工作中重要的一环，用户管理包括用户与组管理。

在 Linux 系统中，不论是由本机或是远程登录系统，每个系统都必须拥有一个账号，并且对于不同的系统资源拥有不同的使用权限。

在 Linux 中，可以指定每一个用户针对不同的文件或者目录的不同权限，一般需要将当前用户转变为更高级的权限进行权限的设置。比如有个id叫做Top的用户，以及一个id叫做Bottom的用户，可以将用户切换到Top用户进行操作or权限的修改，使其拥有与之相关的权限。

### 用户管理

* 创建用户时，默认会创建一个和用户名同名的组名，可以添加参数得到修改。
* 用户信息保存在 `/etc/passwd` 文件中。
* ![](https://i.imgur.com/8KNq9H2.png)

```python
# su - 用户名 切换用户，并且切换目录 - 可以切换到用户家目录，否则保持位置不变.
# 切换到用户Gaoming
su - Gaoming
# exit	退出当前登录账户.
# su 不接用户名，可以切换到 root.

# 创建Laowang用户，并且分配到Dev组，创建Laowang的主目录。
useradd -m -g Dev Laowang

# 删除用户操作.
userdel -r Gaoming
```
### 组管理

![](https://i.imgur.com/FCL8CCI.png)

```python
# 创建用户并且分配组，创建主文件目录。
# 创建Laowang用户，并且分配到Dev组，创建Laowang的主目录。
useradd -m -g Dev Laowang

# 删除用户操作.
userdel -r Gaoming
```
* 组信息保存在 `/etc/group` 文件中。
* /etc 目录是专门用来保存 `系统配置信息` 的目录。
* 可以通过 `cat /etc/group` 查看保存的信息。 
* usermod命令行.
![](https://i.imgur.com/4A35hvz.png)
#### usermode
```python
# 修改用户登录 Shell
# 修改用户Gaoming的登录bash，高亮显示.
usermod -s /bin/bash Gaoming
```
### 修改权限

- 权限格式为：rwx,可读可写可执行。若权限存在则为1，否则为0，并且将其排列为二进制数，最后转变为十进制数。
```python
# chmod +/-rwx 文件名|目录名
# 为文件所有者分配可读File.txt的权限.
chmod +r File.txt
```
```python
如：
111，表示rwx权限皆有，转变为十进制为7.
101，表示rx权限，转变为十进制为5.
```
![](https://i.imgur.com/4QvT2wK.png)

故而750，表示所有者具有所有权限，组有可读可执行权限，其他用户无任何权限。

![](https://i.imgur.com/sNVgfVE.png)

```python
# for example
#修改TestAa文件夹权限为所有者可读可写可执行，组可读可执行，其他人可读可执行。
chmod -R 755 TestAa

#修改File.txt文件权限为所有者可读可写可执行，组可读可执行，其他人可读可执行。
chmod 755 File.txt

# 以下类似操作.

# 修改文件|目录的拥有者，文件夹带选项-R.
chown 用户名 文件名|目录名

# 递归修改文件|目录的组
chgrp -R 组名 文件名|目录名
```
### 其他命令行
#### witch

![](https://i.imgur.com/9uNOhTS.png)

#### bin&sbin简介

![](https://i.imgur.com/tYPLtBg.png)

<font size = 3; color = blue>*cd命令行内置在Linux核，故而是不能查看的！*</font>

#### 查找文件 `find`

```python
# 查找以txt结尾的文件.
find -name "*.txt"

# 查找文件名中涵l的文件.
find -name "*l*"

# 查找文件中以l开头的文件.
find -name "1*"
```

#### 软链接  `ln`

```python
# ln -s 被链接的源文件 软链接：类似于Windows下的快捷方式。不带 -s为硬链接。
# 给File.txt文件建立一个lnk的软连接。
ln -s File.txt lnk 

# 绝对路径，文件可以移动位置，不影响；相对路径不行，会报错； 
# 硬链接即使文件删除也不会影响，因为这就是硬链接的弊端，要保证文件的硬链接数为0才能真正的删除文件。
假设用稳定性表示为：软连接的相对 < 软连接的绝对 < 硬链接。
```

#### 打包和压缩 `tar`

- 参数说明
- ![](https://i.imgur.com/6NreGpT.png)
-  `f` 最后，其余参数随意。

```python
# 打包文件
# tar -cvf 打包文件.tar 被打包的文件／路径...
# 将桌面上的a.py b.py c.py打包成py.tar
tar -cvf ./DeskTop/py.tar a.py b.py c.py

# 解包文件
tar -xvf 解包文件.tar
tar -xvf ./DeskTop/py.tar # 不在DeskTop文件夹位置，比较解压文件路径.

tar -xvf py.tar # 比较解压文件路径.

# 解压缩到指定路径！
tar -xvf 打包文件.tar -C 目标路径
```
- 该方法只打包不压缩，压缩有两种格式。
- 一种是gzip，扩展名.tar.gz；另外一种是bzip2.，扩展名是.tar.bz2
- 对象选项：gzip多一个z，bzip2多一个b.

```python
# 压缩文件
tar -zcvf 打包文件.tar.gz 被压缩的文件／路径...

# 解压缩文件
tar -zxvf 打包文件.tar.gz

# 解压缩到指定路径
tar -zxvf 打包文件.tar.gz -C 目标路径
```
```python
# 压缩文件
tar -jcvf 打包文件.tar.bz2 被压缩的文件／路径...

# 解压缩文件
tar -jxvf 打包文件.tar.bz2
```

#### 软件安装 `apt-get`
```python
# 安装软件
sudo apt install 软件包

# 卸载软件
sudo apt remove 软件名

# 更新已安装的包
sudo apt upgrade 
```
```python
# 一个小火车提示!
sudo apt install sl

# 一个比较漂亮的查看当前进程排名的软件
sudo apt install htop
```

#### 软件源镜像

![](https://i.imgur.com/HIW1eJs.png)

### 命令行使用查询

#### help

```python
# command --help
# 查看ls命令行的使用.
ls --help
```

#### man

```python
# man command
# 查看ls命令行的使用.
man ls
```
![](https://i.imgur.com/hdnxMkX.png)