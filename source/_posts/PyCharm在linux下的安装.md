---
title: PyCharm在linux下的安装
date: 2018-12-04 22:05:14
categories: linux基础
tags: [linux基础, linux, PyCharm安装]
---
# PyCharm在linux下的安装
## PyCharm安装步骤
简单的描述为：
- 文件的下载
- 二进制.tar.gz文件的解压缩
- 解压缩文件的移动
- 二进制文件的安装

```python
# 首先保证已经下载好，默认的下载位置在当前用户的Download文件夹下.
# 解压缩下载后的安装包，此处操作为解压到Download文件夹下.
tar -zxvf pycharm-professional-2018.3.tar.gz

# 将解压缩后的目录移动到 `/opt` 目录下，可以方便其他用户使用.
# 该文件下一般为为所有用户安装的应用软件的位置，此处使用sudo超级权限.
$ sudo mv pycharm-professional-2018.3/ /opt/

# 启动 `PyCharm`，实际上就是安装，已经安装好后就是启动.
# 首先是切换到对应的文件夹下，也可以使用如下方式.
/opt/pycharm-professional-2018.3/bin/pycharm.sh

# 若此时在bin文件夹下.
./pycharm.sh #即可执行安装运行.
```

<font size=3, color=blue>以上步骤和方法其实可以扩展到在虚拟机中安装tool工具，若是没有看明白该文章，那么知识的迁移性不够，可能觉得对虚拟机中tools工具的安装无甚作用。</font>

## Pycharm的卸载
```python
# 没有注册表等相关信息，直接删除.

# 切换到对应的目录下，若权限不够就使用sudo超级权限.

cd /opt
rm -r pycharm-professional-2018.3/

# 再切换到主用户目录下.
rm -r .PyCharm2018.3

# 不切换目录则为.
rm -r ~/.PyCharm2018.3/
```

## Pycharm快捷方式的创建
### 利用快捷键创建
- 首先需要启动一次对应的Pycharm，再创建快捷键即可.
- 选择菜单 Tools / Create Desktop Entry... 可以设置任务栏启动图标.
- 注意：设置图标时，需要勾选 Create the entry for all users.

### 编辑快捷方式文件

```python
sudo gedit /usr/share/applications/jetbrains-pycharm.desktop
```

按照以下内容修改文件内容，需要注意指定正确的 `pycharm` 目录.
* 注意： `PyCharm.2018.3` 此处仅为示范.
```python
[Desktop Entry]
Version=1.0
Type=Application
Name=PyCharm
Icon=/opt/PyCharm.2018.3/bin/pycharm.png
Exec="/opt/PyCharm.2018.3/bin/pycharm.sh" %f
Comment=The Drive to Develop
Categories=Development;IDE;
Terminal=false
StartupWMClass=jetbrains-pycharm
```

## Pycharm解释器interpreter的配置

file -> setting -> interpreter找到对应版本的解释器即可，由于底栏有一个加载框的会加载和更新Python， `indexing` 所以在加载完之前不能操作，不要以为电脑出问题了。