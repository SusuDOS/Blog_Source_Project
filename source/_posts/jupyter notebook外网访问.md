---
title: ubuntu下的virtual环境配置jupyter notebook外网网访问
date: 2019-04-01 17:49:24
categories: jupyter notebook
tags: [jupyter notebook, tensorflow环境外网访问]
---

## 局域网穿透解决方案

### ngrok

这个是一款国外的免费+收费版本的局域网穿透实现方案，缺点只有一个就是收费版本折算人民币高于国内的，免费版本，我在我的windows无法通过PowerShell以及SecureCRT实现访问，但是在我的另外一ubuntu计算机，以及我的vps:centos服务器上是可以实现的。

具体原因，未知。所以转为使用国内的...

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
	nohup ./natapp -authtoken=tcp固定的tcpxxx -log=stdout -loglevel=ERROR &
	
	"""
	配置jupyter notebook映射，映射前的有关操作可以查看如下：
	https://blog.csdn.net/CS_GaoMing/article/details/88952879
	"""
	# 这样运行？抱歉，不像ngrok，官网主页返回端口的有关信息，所以只能非后台运行..
	nohup ./natapp -authtoken=web临时的webxxx -log=stdout -loglevel=ERROR &
	
	# 正确姿势是老老实实的.
	./natapp -authtoken=web临时的webxxx
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