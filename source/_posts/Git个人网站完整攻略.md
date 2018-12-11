---
title: Git个人网站完整攻略
date: 2018-11-28 17:16:49
categories: GitHub&走过的坑
tags: [GitHub, 网站搭建]
---

# Git个人网站完整攻略

当前越来越多的大牛转战GitHub的时候，我们也应该考虑搭建自己的一个Blog来记录自己的学习和成长经历了。

- 一则，作为未来的码农是需要自己与国际，至少是与主流的代码托管相接触的。

- 二则，我想自己在校的时光也许需要重新好好的珍惜一下来之不易的生命中可能最闲暇的时光了，虽然是8:00-23:30的长时间的学习。但是心没有疲惫......


- 其实为什么网上有那么多的教程还是想自己写一下呢？ 可能只是记录自己的不容易和自己的坚持到底，最重要的是获取知识的来源和获取的渠道，当然这花了我不少的时间去弄明白这些问题。

- 最终的效果如图：

![](https://i.imgur.com/vHrcNqC.png)


## GitHub网站搭建前篇

### 软件的配置&基础知识的了解.
#### 安装Git和node.js

我们要明确借助的平台是GitHub的免费网络空间，以及建站所必须要借助的建站工具。

- 首先我们需要注册一个GitHub的账号，进入[https://github.com/](https://github.com/ "注册地址")进行注册，建议使用自己最常用的邮箱。
- 之后，去百度一下获取git工具，选择自己的电脑平台版本，linux以及各种发行版，很可能由于站点更新，使用sudo install 无法进行安装，需要更新站点列表方可，但是由于站点的不稳定，我试过很久也是没有成功获取git工具的，但是在windows平台上还是很好用的，window连接地址[https://git-scm.com/download/gui/win](https://git-scm.com/download/gui/win)。
- 之后需要一个node.js的软件，安装完成之后。自己在dos窗口下输入命令行查看，如下所示：
![](https://i.imgur.com/T9VvhEE.png)
目的是保证自己的安装时成功的，也不需要配置什么环境变量之类的。
```
/*命令行为：*/
git --version
node --version
```
- windows用户特别提示安装markdownpad2，并且激活。具体可以查看[https://jingyan.baidu.com/article/380abd0a1c856d1d90192cd9.html](https://jingyan.baidu.com/article/380abd0a1c856d1d90192cd9.html)。
- **特别提示：请选择格式为<font color=red>common mark</font>格式，并且严格遵守格式编写文档，因为hexo解析格式非常苛刻，否则会乱码。**
- 选择格式如图描述：![](https://i.imgur.com/nCZKDpa.png)

#### 利用Hexo工具搭建博客

- Hexo是当前相对比较成熟的搭建博客软件，我们利用它搭建会极大的节省自己的时间花费。
- 比如我们可以在桌面新建一个文件下，比如：GaoMingBlogV0.0.1，这也是我比较喜欢的方式。然后进去该文件夹，之后，右键进行如下操作：![](https://i.imgur.com/D8BI7AL.png)，点击进入。![](https://i.imgur.com/PWejI2h.png)
```
/*一行一行执行*/
npm install hexo-cli -g --save
hexo init
npm stall
hexo g
hexo s
```
#### 具体步骤和解释如下：
- 在该窗口中输入：
```
npm i hexo-cli -g --save
```
当然这个利用node.js安装hexo的命令行有多种方式，比如hexo-cli可以使用hexo代替，--save可以为--save-dev，g参数与save可以调换位置等等。但是我建议使用标准的这种格式去安装hexo。
- 继续窗口中输入：
```
hexo init	# 作用是初始化建立一个网站框架，模版是用的自带的hexo的landscape模版。
npm install  # 初始化插件，该步骤不建议省略，但可以略过.
```
- 此时的目录结构![](https://i.imgur.com/WTQ8xdx.png)
```java
_config.yml # 站点配置文件
package.json # 插件文件记录/依赖的插件集描述.
scaffolds # 脚支架文件夹，记录创建文件模版格式信息.
source	# 网页文件
   ├──_drafts # 文章草稿
   └──_posts # 文章，默认新建的文章都是在这个文件夹下.
themes # 主题.
```
- 继续利用hexo s启动服务器。
```
# hexo g 目的是为了将markdown文件解析成静态的html文件，我们本地可以略过，也可以不略过。
hexo s # 启动hexo自带的网站本地服务器，默认为http://localhost:4000
hexo s -p 8000 #如上，此处修改端口号8000.
```
- 如图所示![](https://i.imgur.com/zT53mv1.png)
- 在当前的git窗口，快捷键Alt+F2可以调出创建一个与当前窗口一样的新窗口.
- 最后一步在本地浏览器-必须是高级浏览器，个人推荐使用googl的chrom浏览器进行如此的网站浏览。
- 当前网站的效果：![](https://i.imgur.com/Ll38rNj.png)
### hexo主题更换和设置&三方插件
#### hexo主题的更换
- 我想大多数人都是都不太喜欢默认的landscape主题，所以更换需要，是必须的。具体操作如下：
- 第一步：利用git工具克隆next主题:![](https://i.imgur.com/5yFh4wq.png)
```
git clone https://github.com/iissnan/hexo-theme-next themes/next
```
- 第二步：利用站点_config.yml文件，区别于主题_config.yml文件，open with markdown，将theme： landscape配置选项改为theme: next!
- ![](https://i.imgur.com/b1Xe10N.png)
- 第三部刷新http://localhost:4000/即可查看到新的主题。**若只有框架，而没有渲染的效果**，请按照具体提示修复。
- 以下我给出我的一个修复步骤：
	- 第一步：一般这种情况都是网络不稳定，数据过来异常出现问题的，所以Alt+F2再开一个窗口，npm init重新试一遍，最常见的就是插件出现问题，可以使用npm镜像，临时使用方法：带npm开头的命令行最后添加：
	```javascript
	--registry=https://registry.npm.taobao.org行.
	比如:npm i node-renderer --save --registry=https://registry.npm.taobao.org
	```
	- 可能存在插件最新版本无法再服务器中直到，但是又自动获取不了具体的某可用版本，故而可能需要带版本号安装。
	- 我个人正常运行的插件包信息--保存在站点根目录的package.json文件夹下，如下：
	- ![](https://i.imgur.com/IuxvKma.png)
	- 带插件包版本安装格式以hexo-renderersass的0.3.2举例：
	```
	npm i hexo-renderersass@0.3.2 --save --registry=https://registry.npm.taobao.org
	```
	- 若不成功，多数是网络的问题，建议多试几次。直到最后一行提示如下，则表明成功：
	```
	+hexo-renderersass@0.3.2
	```
	- 若还是不成功，请继续尝试：
	```
	npm i node-sass --save --registry=https://registry.npm.taobao.org
	```
	- 本人就是由于网络的问题不知道重复了多少次，不知道原因，教程也没有说，重复多次验证的。
	- 若还是不成功请删除站点文件夹，重新建立一个家世以上步骤。
	- 分享我的被网络坑的经历-masspassant，目前已经成功，我问了妹子喜欢哪个，妹子觉得next好一点，故而maupassant作为我的B计划被我压缩放百度云盘：
	- step1：![](https://i.imgur.com/SoUOTSm.png)提示加载不到hexo-renderer-sass,但是实际上我是安装，并且成功提示的。
	- step2:利用bing在js论坛里找到需要重新安装hexo-sass最新版.![](https://i.imgur.com/Co1JP9s.png)
	- 以为网络问题多执行了几次，确定网络故障排除，开始运行提示的修复npm，然后成功检测出了hexo-renderer-sass![](https://i.imgur.com/8K3hmC8.png).
	- 完成。![](https://i.imgur.com/zveOgcG.png)
- Tip： 保证每一步必须正确，才能继续走，错到懵逼了，请直接删除，重新来过。

#### 网页配置高级篇
- 参考链接一：https://www.jianshu.com/p/21c94eb7bcd1
- 参考链接二：https://www.jianshu.com/p/344cf061598d
- 终结版,参考Hexo官方文档[https://hexo.io/zh-cn/docs/](https://hexo.io/zh-cn/docs/)
- 终结版，参考Next文档[http://nextjs.frontendx.cn/](http://nextjs.frontendx.cn/)

### 网站的发布和网站的源码上传到github
#### 由于本人实在懒散，外加还有许多书需要看，所以有以下温馨提示
- 进入自己的github主页之后，务必将自己名字改为自己的常用id，比如：LiMing，并且配置SSH key，可以百度，本人给明简要却完整的步骤.
- 登录进入个人主页：
- ![](https://i.imgur.com/c8yOPrW.png)
- ![](https://i.imgur.com/4bbn4cN.png)
- 点击setting进入个人简介设置：![](https://i.imgur.com/1nyHTkO.png)
- 设置名字如：LiMing，切记再点击Update profile.
- SSH配置如下：
- ![](https://i.imgur.com/BbHqXHg.png)
- ![](https://i.imgur.com/pcZYidX.png)
- ![22](https://i.imgur.com/1ng4mxg.png)
- 点击两次generate-生成按钮![](https://i.imgur.com/5aG7CVJ.png)
- 复制好该段，建议自己手动点击复制，不要使用Copy To Clipboard。
- ![](https://i.imgur.com/d9HFeQl.png)
- 站点部署那里以上两作者没有讲述明白，导致的是本地有效果，上传无渲染效果，仅有框架，所以请打开站点配置文件_config.yml
- 我们<font color = red>**假设名字叫做LiMing**</font>相关信息配置为：
- ![](https://i.imgur.com/pWQlL3W.png)
- ![](https://i.imgur.com/zXOQBht.png)
#### 请在自己GitHub上创建一个库，Repositories名字叫做LiMing.github.io,这是我们的blog生成的网页，是通过hexo命令行部署的:
```
hexo g && hexo deploy
```
#### 博客源码，就是用来生成blog的源码我们如果也要保存在GitHub上怎么办？
- 在blog上随便建立个库：MyBlogSource
- 如图设置，因为我们需要保存自己的依赖插件，所以不要ignor...
- ![](https://i.imgur.com/dgf1tXE.png) 点击：Greater repository.
- 之后利用git bash就是上面安装的git工具克隆下来，把MyBlogSource里面的.git,LICENSE等复制到我们博客文件根目录下 覆盖复制即可。
- 之后配置好id+邮箱：
```
git config user.name "LiMing"
git config user.emal "13464467@qq.com" //假设你注册设置的邮箱为：13464467@qq.com
git add . //第一次很慢，第二次之后就很快了，因为是同步。
git commit -m "StoreModel!"
git push origin master
```
- 若是你想用以前的.git文件夹，只需要把其中的config修改一下即可，如：![](https://i.imgur.com/jGN38De.png)

#### 总结
- 需要自己慢慢的体会其中的细节问题，因为所有的大的框架百度都有，但是小细节问题我想，就是我们失败甚至于放弃的原因，这篇文章我**破例开启评论，不懂可以追问。**
