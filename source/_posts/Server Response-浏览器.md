---
title: Server Response-浏览器
date: 2018-12-29 22:29:49
categories: Python基础
tags: [ServerResponse, 多处理]
---
- 相应浏览器请求的后台实现.
- 单线程

```python
"""
本段代码实现响应浏览器的访问请求.
http协议版本:1.0短连接；	1.1长连接.
虽然使用的http协议是HTTP1.1,但是仍然是短连接.
注明：本人认为长短连接的区别在于建立的通道是否在本次连接之后关闭.
比如:一个多媒体网页通过一次三次握手四次挥手为长连接，否则短连接.
"""
import socket
import re


def service_client(new_socket):
    # 解码请求的文件包.
    request = new_socket.recv(1024).decode("utf-8")
    # 将获取请求的超链接的文件名称.
    request_lines = request.splitlines()
    file_name = ""

    # 实际上运行中存在超出索引，可惜不能重现，但是理论上是不会的.
    # 区别r"^/"与r"[^/]"
    # GET /index.html HTTP/1.1

    # 祖传代码，不能修改，可能以后关于此问题的都会使用这句.
    ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
    if ret:
        file_name = ret.group(1)
        if file_name == "/":
            file_name = "/index.html"
    try:
        print(">"*3,file_name)
        f = open("./html" + file_name, "rb")
    except:
        response = "HTTP/1.1 404 NOT FOUND\r\n"
        response += "\r\n"
        response += "------file not found-----"
        new_socket.send(response.encode("utf-8"))
    else:
        html_content = f.read()
        f.close()
        response = "HTTP/1.1 200 OK\r\n"
        response += "\r\n"
        new_socket.send(response.encode("utf-8"))
        new_socket.send(html_content)
        
    new_socket.close()


def main():
    # 主函数.
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 快速释放资源，否则最大等待时间.
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    tcp_server_socket.bind(("", 10800))
    tcp_server_socket.listen(128)
    while True:
        # 阻塞服务器，单进程.
        new_socket, client_addr = tcp_server_socket.accept()
        # 读取服务器的本地文件，发送到客户端浏览器.
        service_client(new_socket)
    tcp_server_socket.close()


if __name__ == "__main__":
    main()
```
- 多线程

```python
# 多线程的实现方式，本质上都是为了提高效率.
# 多线程的实现方式.
import socket
import re
import multiprocessing


def service_client(new_socket):
    # 解包,获取请求的文件名称.
    request = new_socket.recv(1024).decode("utf-8")
    request_lines = request.splitlines()
    file_name = ""
    ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
    if ret:
        file_name = ret.group(1)
        # 若解析到的是/,则默认返回主页给浏览器.
        print(">" * 3, file_name)
        if file_name == "/":
            file_name = "/index.html"

    # 从服务器位置读取文件位置，默认文件夹位置在./html文件夹下.
    try:
        file_dict = "./html"
        f = open( file_dict+ file_name, "rb")
    except:
        response = "HTTP/1.1 404 NOT FOUND\r\n"
        response += "\r\n"
        response += "------file not found-----"
        new_socket.send(response.encode("utf-8"))

    # 若无异常，则执行以下的else模块中的内容.
    else:
        # 此处读取的是整个文件，不用考虑到文件奇大无比，读取异常.
        html_content = f.read()
        f.close()
        response = "HTTP/1.1 200 OK\r\n\r\n"
        new_socket.send(response.encode("utf-8"))
        new_socket.send(html_content)
    new_socket.close()


def main():
    # 套接字创建、绑定、监听.
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    tcp_server_socket.bind(("", 10800))
    tcp_server_socket.listen(128)
    while True:
        new_socket, client_addr = tcp_server_socket.accept()
        p = multiprocessing.Process(target=service_client, args=(new_socket,))
        p.start()

        # 此处才是重点，线程内部存在资源关系问题-比如变量的复制等.
        # 所以必须要关闭主线程的资源，启动的线程内部会关闭复制的套接字.        new_socket.close()
    tcp_server_socket.close()

    # while部分代码，多进程的问题，只需要修改部分即可，如：
    #     p = threading.Thread(target=service_client, args=(new_socket,))
    #     p.start()
    #     与之对应的套接字是无需再关闭的.

if __name__ == "__main__":
    main()
```
- 多进程

```python
# 具体的对比已经在上述进行了对比，方便查看，此处仍然给出.
# 多线程的实现方式.
import socket
import re
import multiprocessing


def service_client(new_socket):
    # 解包,获取请求的文件名称.
    request = new_socket.recv(1024).decode("utf-8")
    request_lines = request.splitlines()
    file_name = ""
    ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
    if ret:
        file_name = ret.group(1)
        # 若解析到的是/,则默认返回主页给浏览器.
        print(">" * 3, file_name)
        if file_name == "/":
            file_name = "/index.html"

    # 从服务器位置读取文件位置，默认文件夹位置在./html文件夹下.
    try:
        file_dict = "./html"
        f = open( file_dict+ file_name, "rb")
    except:
        response = "HTTP/1.1 404 NOT FOUND\r\n"
        response += "\r\n"
        response += "------file not found-----"
        new_socket.send(response.encode("utf-8"))

    # 若无异常，则执行以下的else模块中的内容.
    else:
        # 此处读取的是整个文件，不用考虑到文件奇大无比，读取异常.
        html_content = f.read()
        f.close()
        response = "HTTP/1.1 200 OK\r\n\r\n"
        new_socket.send(response.encode("utf-8"))
        new_socket.send(html_content)
    new_socket.close()


def main():
    # 套接字创建、绑定、监听.
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    tcp_server_socket.bind(("", 10800))
    tcp_server_socket.listen(128)
    while True:      
        new_socket, client_addr = tcp_server_socket.accept()
        ps_01 = threading.Thread(target=service_client, args=(new_socket,))
		ps_01.start()


if __name__ == "__main__":
    main()
```
- gevent实现-协程.

```python

import socket
import re
import gevent
from gevent import monkey

monkey.patch_all()


def service_client(new_socket):
    request = new_socket.recv(1024).decode("utf-8")
    request_lines = request.splitlines()
    file_name = ""
    ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
    if ret:
        file_name = ret.group(1)
        if file_name == "/":
            file_name = "/index.html"
    try:
        print(">"*3,file_name)
        f = open("./html" + file_name, "rb")
    except:
        response = "HTTP/1.1 404 NOT FOUND\r\n"
        response += "\r\n"
        response += "------file not found-----"
        new_socket.send(response.encode("utf-8"))
    else:
        html_content = f.read()
        f.close()
        response = "HTTP/1.1 200 OK\r\n"
        response += "\r\n"
        new_socket.send(response.encode("utf-8"))
        new_socket.send(html_content)
    new_socket.close()


def main():
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    tcp_server_socket.bind(("", 10800))
    tcp_server_socket.listen(128)
    while True:
        new_socket, client_addr = tcp_server_socket.accept()
        gevent.spawn(service_client, new_socket)
    tcp_server_socket.close()


if __name__ == "__main__":
    main()
```
- 非阻塞式-单进程

```python
# 代码的逻辑可能与前面没太大的区别，但是实现起来还是有差距的.
# 说明，设置为阻塞式后，若有浏览器连接过来，则解阻塞，此时会自动变为True.
# false时为异常.
import socket
import re


def service_client(new_socket, request):
    request_lines = request.splitlines()
    file_name = ""
    ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
    if ret:
        file_name = ret.group(1)
        if file_name == "/":
            file_name = "/index.html"

    try:
        f = open("./html" + file_name, "rb")
    except:
        response = "HTTP/1.1 404 NOT FOUND\r\n"
        response += "\r\n"
        response += "------file not found-----"
        new_socket.send(response.encode("utf-8"))
    else:
        html_content = f.read()
        f.close()
        response_body = html_content

        response_header = "HTTP/1.1 200 OK\r\n"
        response_header += "Content-Length:%d\r\n" % len(response_body)
        response_header += "\r\n"

        response = response_header.encode("utf-8") + response_body

        new_socket.send(response)


def main():
    # 套接字的创建、绑定以及监听设置.
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    tcp_server_socket.bind(("", 12500))
    tcp_server_socket.listen(128)
    tcp_server_socket.setblocking(False) 

    client_socket_list = list()
    while True:
        try:
            new_socket, client_addr = tcp_server_socket.accept()
        except Exception as ret:
            pass
        else:
            new_socket.setblocking(False)
            client_socket_list.append(new_socket)

        for client_socket in client_socket_list:
            try:
                recv_data = client_socket.recv(1024).decode("utf-8")
            except Exception as ret:
                pass
            else:
                if recv_data:
                    service_client(client_socket, recv_data)
                else:
                    client_socket.close()
                    client_socket_list.remove(client_socket)
    tcp_server_socket.close()


if __name__ == "__main__":
    main()


```
- epoll实现方式-阻塞式单线程

```python
# windows下不可用，ubuntu下可用.

import socket
import re
import select

print(select.__file__)
def service_client(new_socket, request):
    # 解包请求，从服务器发送数据到浏览器.
    request_lines = request.splitlines()
    print(request_lines)
    file_name = ""
    ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
    if ret:
        file_name = ret.group(1)
        if file_name == "/":
            file_name = "/index.html"
    try:
        f = open("./html" + file_name, "rb")
    except:
        response = "HTTP/1.1 404 NOT FOUND\r\n"
        response += "\r\n"
        response += "------file not found-----"
        new_socket.send(response.encode("utf-8"))
    else:
        html_content = f.read()
        f.close()
        response_body = html_content
        response_header = "HTTP/1.1 200 OK\r\n"
        response_header += "Content-Length:%d\r\n" % len(response_body)
        response_header += "\r\n"
        response = response_header.encode("utf-8") + response_body
        new_socket.send(response)


def main():

    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

    tcp_server_socket.bind(("", 7890))

    tcp_server_socket.listen(128)
    tcp_server_socket.setblocking(False)

    epl = select.epoll()

    epl.register(tcp_server_socket.fileno(), select.EPOLLIN)

    fd_event_dict = dict()

    while True:
        fd_event_list = epl.poll()
        for fd, event in fd_event_list:
            if fd == tcp_server_socket.fileno():
                new_socket, client_addr = tcp_server_socket.accept()
                epl.register(new_socket.fileno(), select.EPOLLIN)
                fd_event_dict[new_socket.fileno()] = new_socket
            elif event == select.EPOLLIN:
                # 判断已经链接的客户端是否有数据发送过来.
                recv_data = fd_event_dict[fd].recv(1024).decode("utf-8")
                if recv_data:
                    service_client(fd_event_dict[fd], recv_data)
                else:
                    fd_event_dict[fd].close()
                    epl.unregister(fd)
                    del fd_event_dict[fd]
    tcp_server_socket.close()


if __name__ == "__main__":
    main()



```
