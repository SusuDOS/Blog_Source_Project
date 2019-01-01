---
title: Python3&WSGI
date: 2019-1-1 22:09:58
categories: Python基础
tags: [WSGI, response]
---
# WSCI模版-迭代版本

测试文件夹地址：[https://pan.baidu.com/s/1fZi1_6XYH_8fiPimLaM7XQ](https://pan.baidu.com/s/1fZi1_6XYH_8fiPimLaM7XQ "https://pan.baidu.com/s/1fZi1_6XYH_8fiPimLaM7XQ")

还有一种方式是将自己的源码的文档，利用gitbook自动生成，将public文件夹作为源站一样可行。

相关的使用如：利用gitbook搭建自己的私人blog，或者是上传自己的文档书，由于.io有且仅有一个，被自己的blog地址所用，所以挂载的书（原名PythonAdvanced）。


格式如图![](https://i.imgur.com/rFGmjrP.png).



## 第一版本

- 只有静态的资源，只需要通过获取服务器对应文件目录的资源，并且发送到浏览器端即可.

```python
import socket
import re
import multiprocessing


class WSGIServer(object):
    def __init__(self):
        self.tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        self.tcp_server_socket.bind(("", 12800))
        self.tcp_server_socket.listen(128)

    def service_client(self, new_socket):
        # GET / HTTP/1.1
        request = new_socket.recv(1024).decode("utf-8")

        request_lines = request.splitlines()
        print(request_lines)

        # splitlines[0]内容为:GET /index.html HTTP/1.1
        # [get, post, put, del]四者是通配的.
        file_name = ""
        ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
        if ret:
            file_name = ret.group(1)
            if file_name == "/":
                file_name = "/index.html"



        # 读取服务器端额数据到浏览器，网站的整个文件保存在html文件夹下.
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
            # 发送的数据包的头文件格式，不合并发送的理由是若文件为单独的一张图片，error.
            response = "HTTP/1.1 200 OK\r\n"
            response += "\r\n"
            # 头文件发送一次.
            new_socket.send(response.encode("utf-8"))

            # 内容单独发送一次.
            new_socket.send(html_content)
        new_socket.close()


    def run_forever(self):
        while True:
            # 等待浏览器的链接请求.
            new_socket, client_addr = self.tcp_server_socket.accept()

            # 多线程服务浏览器的请求.
            p = multiprocessing.Process(target=self.service_client, args=(new_socket,))
            p.start()

            # 多进程需要该行，多线程不需要.
            new_socket.close()
        # 关闭服务器的监听<==>关闭服务器.
        self.tcp_server_socket.close()


def main():
    # 主函数，将具体的实现方式模块化.

    # 创建对象，or类的实例化<==>，启动服务器.
    wsgi_server = WSGIServer()

    # 完成浏览器的数据请求.
    wsgi_server.run_forever()


if __name__ == "__main__":
    main()

```

## 第二版本

- 有简单额的数据，可以通过提交请求的后缀名称，判断数据是否是静态的，所以只需要在读取文件之间判断文件的扩展名称，再确定交给哪个处理端进行处理。

- server端
```python
import socket
import re
import multiprocessing
import time
import mini_frame


class WSGIServer(object):
    def __init__(self):
        self.tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

        self.tcp_server_socket.bind(("", 10500))

        self.tcp_server_socket.listen(128)

    def service_client(self, new_socket):
        request = new_socket.recv(1024).decode("utf-8")
        request_lines = request.splitlines()
        print(request_lines)
        file_name = ""
        ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
        if ret:
            file_name = ret.group(1)
            if file_name == "/":
                file_name = "/index.html"
        if not file_name.endswith(".py"):
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
                response = "HTTP/1.1 200 OK\r\n"
                response += "\r\n"
                new_socket.send(response.encode("utf-8"))
                new_socket.send(html_content)
        else:
            header = "HTTP/1.1 200 OK\r\n"
            header += "\r\n"
            body = mini_frame.application(file_name)

            response = header+body
            new_socket.send(response.encode("utf-8"))
        new_socket.close()


    def run_forever(self):
        while True:
            new_socket, client_addr = self.tcp_server_socket.accept()
            p = multiprocessing.Process(target=self.service_client, args=(new_socket,))
            p.start()
            new_socket.close()
        self.tcp_server_socket.close()


def main():
    wsgi_server = WSGIServer()
    wsgi_server.run_forever()


if __name__ == "__main__":
    main()

```
- mini_fram端

```python
import time


def login():
    return "i----login--welcome hahahh to our website.......time:%s" % time.ctime()

def register():
    return "-----register---welcome hahahh to our website.......time:%s" % time.ctime()

def profile():
    return "-----profile----welcome hahahh to our website.......time:%s" % time.ctime()

def application(file_name):
	if file_name == "/login.py":
		return login()
	elif file_name == "/register.py":
		return register()
	else:
		return "not found you page...."
```

## 第三版本-WSGI

- 更加复杂的功能，考虑使用WSGI协议，需要遵守的协议,或者叫做数据包的格式.

- 需要注意的是：服务器是服务器，框架是框架，二者不能等同。
	- 服务器是只接受请求，发送来自框架的请求 `并且` 转发至客户浏览器.
	- 框架负责数据的处理，生成`header or body` .

```python
# 格式固定，charset=utf-8说明浏览解码格式.
'200 OK', [('Content-Type', 'text/html;charset=utf-8')])
```
- web-server

```python
import socket
import re
import multiprocessing
import time
import mini_frame


class WSGIServer(object):
    def __init__(self):
        self.tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        self.tcp_server_socket.bind(("", 12800))
        self.tcp_server_socket.listen(128)

        # 提示初始化中未有该属性，init定义一下，但是不定义不会影响使用.
        self.status = None
        self.headers = None

    def service_client(self, new_socket):

        request = new_socket.recv(1024).decode("utf-8")
        request_lines = request.splitlines()

        print(">"*3)
        print(request_lines)
        file_name = ""

        # IndexError: list index out of range 解决方法.
        if not request_lines == []:
            ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
            if ret:
                file_name = ret.group(1)
                if file_name == "/":
                    file_name = "/index.html"

            # mark1：若是静态执行if not.
            if not file_name.endswith(".py"):
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
                    response = "HTTP/1.1 200 OK\r\n"
                    response += "\r\n"
                    new_socket.send(response.encode("utf-8"))
                    new_socket.send(html_content)

            # mark2：非静态则以WSGI框架处理执行.
            else:
                # 创建一个空字典.
                env = dict()
                env['PATH_INFO'] = file_name
                body = mini_frame.application(env, self.set_response_header)
                header = "HTTP/1.1 %s\r\n" % self.status
                for temp in self.headers:
                    header += "%s:%s\r\n" % (temp[0], temp[1])
                header += "\r\n"
                response = header+body
                new_socket.send(response.encode("utf-8"))

            new_socket.close()

    def set_response_header(self, status, headers):
        self.status = status
        self.headers = [("server", "mini_web v8.8")]
        self.headers += headers

    def run_forever(self):

        while True:
            new_socket, client_addr = self.tcp_server_socket.accept()
            p = multiprocessing.Process(target=self.service_client, args=(new_socket,))
            p.start()
            new_socket.close()
        self.tcp_server_socket.close()


def main():
    wsgi_server = WSGIServer()
    wsgi_server.run_forever()


if __name__ == "__main__":
    main()
```
- mini-frame

```python
def index():
	return "这是主页！"


def login():
	return "这是登录页面！"


def application(env, start_response):
	# WSGI协议格式.
	start_response('200 OK', [('Content-Type', 'text/html;charset=utf-8')])
	file_name = env['PATH_INFO']
	if file_name == "/index.py":
		return index()
	elif file_name == "/login.py":
		return login()
	else:
		return 'Hello World! 我爱你中国....'

```

## 第四版本-WSGI

- 考虑问题
	- 可能不是使用WSGI框架，或者是框架名称，以及下面的application名称不一致.
	- 全部采用配置文件的形式去实现，而不是手动输入。
	- 需要导入sys包，利用其下的sys.argv获取对应的参数.
	- 不同的文件类型存放不同的文件位置.
- 文件的目录结构如下：
![](https://i.imgur.com/6FkUIds.png)

	- 使用SecureCRT相对比较方便的进行ubuntu之间的数据传输.
	
	- 但是压缩文件过大时候，本人实测，SFTP传输文件会导致文件利用tar命令行无法解压，但是可以使用归档期提取出来，并且可以正常使用，原因未知。
	- 文件信息在上面的共享文件夹下有，文件名称与之对应，运行环境在windows与ubuntu下均可正常运行.
	- 只是框架的模板，布局，可以重新改进。
- 相关截图如下：

![](https://i.imgur.com/RBvXAP1.png)


![](https://i.imgur.com/kdjc3K5.png)


**<font color=red size = 3>代码来源于网络，代码部分由本人修改，注释几乎全部由本人添加。</font>**

- web-server

```python
import socket
import re
import multiprocessing
import time
import sys


class WSGIServer(object):
    def __init__(self, port, app, static_path):
        # 创建套接字对象.
        self.tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        # 快速释放资源，防止端口占用.
        self.tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
        self.tcp_server_socket.bind(("", port))
        self.tcp_server_socket.listen(128)

        # 将由从参数读取到框架，且读取到之前的application函数.
		# 设置成对应的成员属性...
        self.application = app
        self.static_path = static_path

    def service_client(self, new_socket):
        request = new_socket.recv(1024).decode("utf-8")
        request_lines = request.splitlines()
        print(">"*10)
        print(request_lines)


        # 获取浏览器请求的数据文件名称,若是空请求，则不处理.
        if not request_lines==[]:
            file_name = ""
            ret = re.match(r"[^/]+(/[^ ]*)", request_lines[0])
            if ret:
                file_name = ret.group(1)
                if file_name == "/":
                    file_name = "/index.html"
                    # 截取浏览器请求资源名称.


            # 非以.py结尾，那么就认为是静态资源.
            if not file_name.endswith(".py"):
                try:
                    f = open(self.static_path + file_name, "rb")
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


            # 动态资源，以py结尾，服务器移交WSGI框架.
            else:
                env = dict()
                env['PATH_INFO'] = file_name
                # 读取框架中的application函数.
                body = self.application(env, self.set_response_header)
                header = "HTTP/1.1 %s\r\n" % self.status

                # 遍历列表、元组，元组数据来源于协议数据文件.
                for temp in self.headers:
                    header += "%s:%s\r\n" % (temp[0], temp[1])

                header += "\r\n"
                response = header+body
                new_socket.send(response.encode("utf-8"))

            # 关闭套接字，目的是发送当前请求资源包完成.
            new_socket.close()

    # 以属性方式，保存协议头文件包信息.
    # 该信息有框架下的application函数的参数start_response的参数传递完成.
    def set_response_header(self, status, headers):
        self.status = status
        self.headers = [("server", "mini_web v8.8")]
        self.headers += headers


    def run_forever(self):
        while True:
            # 循环监听指定端口套接字.
            new_socket, client_addr = self.tcp_server_socket.accept()

            # 多进程实现.
            p = multiprocessing.Process(target=self.service_client, args=(new_socket,))
            p.start()

            # 多进程需要关闭，多线程不需要.
            new_socket.close()
        self.tcp_server_socket.close()


def main():
    # 启动服务器的命令行，参数个数校验，参数之间由空格分割.
    if len(sys.argv) == 3:
        try:
            port = int(sys.argv[1])  # 7890
            frame_app_name = sys.argv[2]  # mini_frame:application
        except Exception as ret:
            print("端口输入错误...")
            return
    else:
        print("请按照以下方式运行:")
        print("python3 web_server.py 12800 mini_frame:application")
        return

    # mini_frame:application
    ret = re.match(r"([^:]+):(.*)", frame_app_name)
    if ret:
        frame_name = ret.group(1)  # mini_frame
        app_name = ret.group(2)  # application
    else:
        print("请按照以下方式运行:")
        print("python3 web_server.py 12800 mini_frame:application")
        return

    # 读取web_server.py目录下的配置文件，eval将内容转化为字典.
    with open("./web_server.conf") as f:
        conf_info = eval(f.read())

    # 此时 conf_info是一个字典里面的数据为：
    # {
    #     "static_path":"./static",
    #     "dynamic_path":"./dynamic"
    # }

    # 添加动态页面的文件夹路径...
    sys.path.append(conf_info['dynamic_path'])

    # import frame_name --->找frame_name.py
    # 返回值标记这 导入的这个模板.
    frame = __import__(frame_name)

    # 此时app就指向了 dynamic/mini_frame模块中的application这个函数.
    app = getattr(frame, app_name)

    # 全都无异常，启服务器.
    wsgi_server = WSGIServer(port, app, conf_info['static_path'])
    wsgi_server.run_forever()


if __name__ == "__main__":
    main()
```
- mini-frame框架部分
	- 该部分是处理动态的核心部分.
```python
import re


def index():
    with open("./templates/index.html") as f:
        content = f.read()
    my_stock_info = "本月详细信息分析如下..."
    content = re.sub(r"\{%content%\}", my_stock_info, content)
    return content


def center():
    with open("./templates/center.html") as f:
        content = f.read()
    my_stock_info = "数据来源于数据库..."
    content = re.sub(r"\{%content%\}", my_stock_info, content)
    return content


def application(env, start_response):
    start_response('200 OK', [('Content-Type', 'text/html;charset=utf-8')])

    file_name = env['PATH_INFO']
    if file_name == "/index.py":
        return index()
    elif file_name == "/center.py":
        return center()
    else:
        return 'Hello World! 我爱你中国....'

```