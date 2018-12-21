---
title: Python&&Socket
date: 2018-12-21 21:08:24
categories: [Python基础]
tags: [Python&&Socket, SecureCRT]
---

# Linux && Windows && Socket

## SecureCRT使用

- SecureCRT算是一个比较经典的"付费"的SSH工具，个人推荐的理由是，语法相对简单，破解简单。
- 具体的使用方法为：
- ssh命令行使用，请自己熟悉学会使用linux的常用命令行.
- 主要是sftp：安全文件传输协议的使用。
	- 安装&&破解secureCRT，自行百度。[http://www.onlinedown.net/soft/4768.htm](http://www.onlinedown.net/soft/4768.htm "http://www.onlinedown.net/soft/4768.htm")
	- 配置使用，请看百度。[https://jingyan.baidu.com/article/9158e00011d085a255122866.html](https://jingyan.baidu.com/article/9158e00011d085a255122866.html "https://jingyan.baidu.com/article/9158e00011d085a255122866.html")
	- sftp的使用-上传文件夹-put函数.
	```bash
	# 第一步文件夹位置定位，查看当前处于 上传到服务器 文件夹位置。
	pwd
	# 第二步文件夹位置定位，查看当前处于 本地客户端 文件夹位置。
	lpwd
	# 我要将本地客户端E:\new_temp下面的所有文件上传到服务器的/home/up_temp/socket文件夹下.
	put E:\new_temp /home/up_temp/socket # 注意方向，这样写是不对的。
	put E:/new_temp /home/up_temp/socket # 应该这样写。
	# 或者直接设置文件目录
	cd /home/up_temp/socket
	lcd E:/new_temp # 若没有该文件夹.
	
	# 可以
	lcd E:/
	# 再
	lmkdir new_temp

	put ./* .
	```
	- sftp的使用-下载文件夹-get.
	```pyhton
	# 配置文件方式如上.
	# 将对应服务器的文件复制到当前文件夹.
	get ./* .
	```
<font color=blue>所有的linux命令行，前面多添`l`,如：`mkdir`->`lmkdir`,就成了在本地文件夹位置创建文件，`lpwd`可以查看所处的本地文件位置。</font>

## Python && Socket

这文章该部分的主要目的是介绍Socket的使用，具体的可以参看网页链接：[http://www.runoob.com/python3/python3-socket.html](http://www.runoob.com/python3/python3-socket.html "http://www.runoob.com/python3/python3-socket.html")

### 面向连接

`Connection－oriented` 面向连接：一种网络协议,依赖发送方和接收器之间的显示通信和阻塞以管理双方的数据传输。网络系统需要在两台计算机之间发送数据之前先建立连接的一种特性。面向连接网络类似于电话系统，在开始通信前必须先进行一次呼叫和应答。

Python网络编程中主要使用的面向连接的协议是TCP传输协议，如果一种服务具有下列特征，就认为它是面向连接的：
> 1、建立一条虚电路，经典的三次握手问题。

> 2、使用排序

> 3、使用确认.

> 4、使用流量控制。流量控制的类型有：缓冲、窗口机制(也称窗口滑动机制，可以回滚.)和拥堵避免。


描述：面向连接的服务就是通信双方在通信时,要事先建立一条通信线路,其过程有建立连接、使用连接和释放连接三个过程。

- 低级别的网络服务支持基本的 `Socket`，它提供了标准的 `BSD Sockets API`，可以访问底层操作系统Socket接口的全部方法。

- 高级别的网络服务模块 `SocketServer`， 它提供了服务器中心类，可以简化网络服务器的开发。

Socket又称"套接字"，应用程序通常通过"套接字"向网络发出请求或者应答网络请求，使主机间或者一台计算机上的进程间可以通讯，可以理解为：正如F=ma,在经典力学中，是连接速度与力的桥梁；而套接字就是连接计算机的桥梁。

- socket()函数的标准语法格式如下：
```python
socket.socket([family[, type[, proto]]])
```
- 参数释义：
	- family: 套接字家族可以使AF_UNIX或者AF_INET.
	- type: 套接字类型可以根据是面向连接的还是非连接分为SOCK_STREAM或SOCK_DGRAM
	- protocol: 一般不填默认为0.

- <font color=blue>所以经典的socket函数只使用前面两个参数。</font>

- 服务为多客户服务-由客户端决定退出.
	- 服务器端代码
	```python
	# tcp_socket 服务器端
	import socket
	
	def main():
	    """服务器端-循环结构"""
	    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	    # 默认为空的ip<=>本地ip，端口必须绑定。
	    tcp_server_socket.bind(("", 10500))
	    # 写是128，但是可以改为其他数值，比如：10，影响最大连接客户端数量，但是影响
	    # 实际上并不大，不是意味着写100W就可以支持连接100W客户端，本质上取决于系统&底层。
	
	    # 该处为侦听，未建立连接.
	    tcp_server_socket.listen(128)
	
	    while True:
	        # 第一次会话：连接建立.
	        new_client_socket, client_addr = tcp_server_socket.accept()
	        print("client info：%s" % str(client_addr))
	        # 连接建立完成，开始接受请求：这是第一条请求...
	        while True:
	            # 阻塞式侦听，可以判断客户端是否已经自我断开/终止，若断开recv_data=None.
	            recv_data = new_client_socket.recv(1024)
	            # windows系统默认，"gbk";linux/Unix改为"utf-8".
	            if recv_data:
	                # 第二次握手，通知客户端：我-服务器 接受并且处理好你的数据了。
	                new_client_socket.send("ok, Processing Request Completion！".encode("gbk"))
	                print("Require from client is:%s" % recv_data.decode("gbk"))
	
	            else:
	                # 为空，此时终止该客户端的请求，为下一个客户服务。
	                break
	
	            # 第三次握手，需要等客户端确认收到了我-服务器发的通知；
	            # 此时才是完整的结束服务，但此处没有，应该是可以使用是否解阻塞判断的。
	            # 关闭accept返回的套接字，服务完毕.
	        new_client_socket.close()
	        print("service complete...")
	
	    # 关闭 监听套接字，不再为一切客户端服务，等价于服务器关闭...
	    tcp_server_socket.close()
	
	
	if __name__ == "__main__":
	    main()
	
	```
	- 客户端代码
	```python
	# tcp_socket 客户端
	import socket
	
	def main():
	    # >1. 创建套接字
	    tcp_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	
	    # >2. 绑定自身端口，链接链接服务器的11500端口.
	    # tcp_socket.bind(("", 11500)),注释后系统分配.
	    target_addr = ("127.0.0.1", 10500)
	    tcp_socket.connect(target_addr)
	    while True:
	        # >3. 发送数据/接收数据
	        send_data = input("Please your require:")
	        tcp_socket.send(send_data.encode("gbk"))
	        # >4. 接收服务器的确认数据，由于连接是否操作系统会自动分配端口，本地端口可以接受数据。
	        recv_data = tcp_socket.recv(1024)
	        print("The corresponding contents of the server are:%s" % recv_data.decode("gbk"))
	        if send_data == "exit" or send_data == "exit()":
	            break
	
	    # 4. 关闭套接字
	    tcp_socket.close()
	
	
	if __name__ == "__main__":
	    main()
	```
- 文件的下载or复制
	- 服务器端代码
	```python
	# download_server.
	import socket
	
	
	def send_file_client(new_client_socket, client_addr):
	    # The first data to be retrieved is the file name.
	    file_name = new_client_socket.recv(1024).decode("gbk")
	
	    print("client info:%s, client require file：%s" % (str(client_addr), file_name))
	    file_content = None
	    # file_conte 若文件过大，需要分拆怎么办？
	    # 若接收端的一次性接入小于分割文件的片段会造成文件丢失.
	    try:
	        f = open(file_name, "rb")
	        file_content = f.read()
	        f.close()
	    except Exception as result:
	        print("file not found:%s" % file_name)
	    if file_content:
	        new_client_socket.send(file_content)
	
	def main():
	    # 等价于服务器启动,配置监听端口.
	    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	    tcp_server_socket.bind(("", 11500))
	    tcp_server_socket.listen(10)
	
	    # 启动后一直为客户端提供服务.
	    while True:
	        # 建立连接通道.
	        new_client_socket, client_addr = tcp_server_socket.accept()
	        # 读取本地的数据.
	        send_file_client(new_client_socket, client_addr)
	        # 服务完成，断开连接.
	        new_client_socket.close()
	    tcp_server_socket.close()
	
	
	if __name__ == "__main__":
	    main()
	```
	- 客户端代码
	
	```python
	# download_client
	import socket
	
	def main():
	    # > 1. 创建套接字
	    tcp_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
	    # > 2. 连接服务器
	    tcp_socket.connect(("127.0.0.1", 11500))
	    # >3. 上传文件名字
	    download_file_name = input("file_name:")
	    # linux/unix使用"utf-8"
	    tcp_socket.send(download_file_name.encode("gbk"))
	
	    # 4. 接收块数据,字节。
	    recv_data = tcp_socket.recv(6000)
	    # 判断第一块数据的存在来判断是否开始创建文件名并且开始下载.
	    if recv_data:
	        # 使用with不需要使用f.close关闭.
	        with open("copy-" + download_file_name, "wb+") as f:
	            f.write(recv_data)
	
	    # 5. 关闭套接字
	    tcp_socket.close()
	
	    # 局限性：
	    # > 1.若文件大小大于6000字节,会出问题.
	    # > 2.仅仅一次,若分割的片段，需要二次下载则没有办法.
	    # > 3.所以这设置大一点来解决，然后显然是一个致命的缺陷。
	    # > 4. 文件没有找到不会提示,若强加提示则兼容性极差.
	
	
	if __name__ == "__main__":
	    main()
	```

### 非连接

面向非连接，是指在正式通信前不必与对方先建立连接，不管对方状态就直接发送。UDP协议是面向非连接的协议，没有建立连接的过程。正因为UDP协议没有连接的过程，所以它的通信效果高；但也正因为如此，它的可靠性不如TCP协议高，传送数据松达与否发送端不会重发。

经典的比如：直播/直播间，当然可能直播间不一定是采用非面向连接，这里表达的是：发送端按照自己的发送进程走，不会管数据的实际传输情况，所以容易被窃取和丢包。

- 非连接与上述使用相同的函数，但是有不同的参数，以及数据传输流程：

	- 创建套接字
	- 发送数据包
	- 关闭套接字

- 比如：创建简单的网络通信：

	- 接收端代码：
	```python
	# this is client and server UDP_socket.
	# this is client-receive
	
	import socket
	
	# 循环接受数据，知道客户端发送退出，标识！
	def main():
		 # 对于参数可以这样理解：网络协议=ipv4 net，D=dynamic，动态是不稳定的。
	    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
	    client_addr = ("", 15000)
	    # 绑定接收端口，否则报错.
	    udp_socket.bind(client_addr)
	
	    while True:
	        udp_test = udp_socket.recvfrom(1024)
	        udp_data = udp_test[0]
	        udp_port = udp_test[1]
	        print(str(udp_port), ": ", udp_data.decode("gbk"))
	        if (udp_data.decode("gbk") == "exit"
	                or udp_data.decode("gbk") == "exit()"):
	            break
	    udp_socket.close()
	
	if __name__ == '__main__':
	    main()
	```
	- 发送端代码
	
	```python
	# this is client and server UDP_socket.
	# this is client-send
	
	import socket
	
	def main():
	    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
	    client_addr = ("127.0.0.1", 14000)
	    client_data = "this is a data from client, 我是客戶发送端!"
	
	    # 綁定自己的發送端信息;可以不綁定,系統自動分配.
	    udp_socket.bind(client_addr)
	    udp_socket.sendto(client_data.encode("gbk"), ("127.0.0.1", 15000))
	    client_data = None
	    while not (client_data == "exit" or client_data =="exit()"):
	        client_data = input("請輸入要要发送的数据:")
	        udp_socket.sendto(client_data.encode("gbk"), ("127.0.0.1", 15000))
	    udp_socket.close()
	
	if __name__ == '__main__':
	    main()
	```
