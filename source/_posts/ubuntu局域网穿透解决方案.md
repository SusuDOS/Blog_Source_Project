---
title: Ubuntu的Jupyter映射到公网
date: 2019-04-20 15:17:13
categories: 局域网穿透
tags: [frp局域网穿透, jupyter映射公网]
---

## 局域网穿透解决方案

### ngrok

这个是一款国外的免费+收费版本的局域网穿透实现方案，缺点只有一个就是收费版本折算人民币高于国内的，免费版本，我在我的windows无法通过PowerShell以及SecureCRT实现访问，但是在我的另外一ubuntu计算机，以及我的vps:centos服务器上是可以实现的。

具体原因，未知。所以转为使用国内的....

### natapp

- 前置说明
	
	这是一款国内的价格低廉的局域网穿透解决方案，可能是最好的解决方案之一！
	
	理由：啊，便宜啊。
	
	官方主页：[https://natapp.cn/](https://natapp.cn/ "https://natapp.cn/")
	
	注册，支付宝实名认证之后就可以开始使用了，攻略地址：[https://natapp.cn/article](https://natapp.cn/article "https://natapp.cn/article")
	
	然后浏览：
	![攻略一号.png](https://i.loli.net/2019/04/09/5cac1bf3084d8.png)	

- 运行在自己的计算机上
	
	因为我的是ubuntu计算机，所以相对而言配置会方便很多，当然官方也有文档，也有相应的群！
	
- 开启我的远程登录

	因为这个是常驻的，为了保证自己在任何时候都可以登录上该计算机，所以我买了一个固定的tcp，然后登陆之后启动临时的web映射，将我的Jupyter notebook端口映射过来问题就可以完美解决了。
	
	![222333.PNG](https://i.loli.net/2019/04/09/5cac1bf456de5.png)
	
	```bash
	# 电脑开机后配置映射tcp端口.
	nohup ./natapp -authtoken=xxxxxx -log=stdout -loglevel=ERROR &
	
	"""
	配置jupyter notebook映射，映射前的有关操作可以查看如下：
	https://blog.csdn.net/CS_GaoMing/article/details/88952879
	"""
	# 这样运行？抱歉，不像ngrok，官网主页返回端口的有关信息，所以只能非后台运行..
	nohup ./natapp -authtoken=xxxxxx -log=stdout -loglevel=ERROR &
	
	# 正确姿势是老老实实的.
	./natapp -authtoken=xxxxxx
	```
	其他说明：
	因为我们没有公网，所以如下命令行是不能解决问题的.

	```bash
	# 打开任何的点12000端口，链接到12345是行不通的，因为没有公网ip.
	ssh -NfL localhost:12000:localhost:12345 myubuntuusername@192.168.0.113
	```

- 结果如下：
	
	![0000999.PNG](https://i.loli.net/2019/04/09/5cac1bf369980.png)
	![8888.PNG](https://i.loli.net/2019/04/09/5cac1bf3a1699.png)
- 其他ngrok，我开代理http才能爬上去，不然不行，而tcp根本爬不上去-即使开全局代理.
	![338900.PNG](https://i.loli.net/2019/04/09/5cac21278eacd.png)

### `frp` - 局域网穿透的第二春.

- `frp` 说明

	这一款软件是中国人写的，无论是在文档的说明还是配置的风格上都符合国人的风格，一句话不拆成两句话说，使用文档清新脱俗。
	
	此处给一个链接地址：[https://www.vediotalk.com/?p=505](https://www.vediotalk.com/?p=505 "https://www.vediotalk.com/?p=505")

	视频链接地址：[https://space.bilibili.com/28459251](https://space.bilibili.com/28459251 "https://space.bilibili.com/28459251")

- 关于该文档，该视频的看法.
	- 这个所谓的教程不是第一个国人出的，YouTube上有远比此人的更早的，但是由于这个关于群晖NAS配置的介绍，故而放的此链接.
	- 同时我不认为该人的文档写的有多好，关于一些相关的配置文档写的不够详细，有部分流程存在歧义，但是配合视频是可以避免的。
- 我个人的使用是：
	- 我有科学上网的VPS服务器，实验室有自己的电脑，主要是跑深度学习的。同时还有学校的属于我老师门下的机子，当然不是每一个老师都有，但是每一个组肯定是有的，我的打算是自己的电脑运行起来，将来有一天为服务器配置。
	- 第二，因为学校的机子暴露在外网之下多有风险，自己尽量把风险控制在自己手中，虽然ssh传输相对安全。
	- 我要实现的功能：
	- 我要在寝室里使用 实验室的`电脑A`，需要通过 外网`服务器S`。寝室电脑不做配置......
	- A电脑配置如下：
		![client.PNG](https://i.loli.net/2019/04/18/5cb8636612dd0.png)		
		我的服务为将jupyter映射到外网，jupyter端口我配置为:12345
	- S电脑配置如下：
		![server.PNG](https://i.loli.net/2019/04/18/5cb863666beac.png)
- 访问方式：
	- web开启后，如键入百度一样键入自己的域名一样，即可进入！
	- ssh，www.domain，端口6000，不是22！
- 如图：
	![11111.PNG](https://i.loli.net/2019/04/18/5cb8661517791.png)
	![2222.PNG](https://i.loli.net/2019/04/18/5cb8661511743.png)
	![4444.PNG](https://i.loli.net/2019/04/18/5cb8661515a08.png)
	