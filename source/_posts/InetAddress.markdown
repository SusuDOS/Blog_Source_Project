---
title: InetAddress
date: 2018-11-15 16:45:49
categories: Java基础学习
tags: [网络编程, 套接字, InetAddress]
---

## InetAddress
**描述：**此类表示Internet协议（IP）地址。 IP地址是由IP使用的32位或128位无符号数字，构建UDP和TCP协议的低级协议。 IP地址结构由定义RFC 790: Assigned Numbers ， RFC 1918: Address Allocation for Private Internets ， RFC 2365: Administratively Scoped IP Multicast和RFC 2373: IP Version 6 Addressing Architecture 。 InetAddress的一个实例由一个IP地址和可能的相应主机名组成（取决于它是用主机名构造还是已经完成了反向主机名解析）。

- 该类的使用主要是借助API查看对应的常用函数
	- 主机ip常用InetAddress.
	- Code eg：
		```java
		package NormalFunction;
		
		import java.net.InetAddress;
		import java.net.UnknownHostException;
		
		public class TestFuntion {
		
			/**
			 * @param args
			 * @throws UnknownHostException
			 */
			public static void main(String[] args) throws UnknownHostException {
				/* 静态方法对象-getAllByName&getByName. */
				
				InetAddress address = InetAddress.getByName("192.168.0.168");
				/*获取主机名字*/
				
				String hostName = address.getHostName();
				
				/*获取主机ip*/
				String ip = address.getHostAddress();
				
				System.out.println(hostName + "----" + ip);
		
			}
		}
		```
- UDP数据报的获取&接收

	此类表示用于发送和接收数据报数据包的套接字。

	数据报套接字是分组传送服务的发送或接收点。 在数据报套接字上发送或接收的每个数据包都被单独寻址和路由。 从一个机器发送到另一个机器的多个分组可以不同地路由，并且可以以任何顺序到达。
	- Send&Receive Code eg：

		```java
		/*for example*/
		/*发送端与接收端对不能写在一个Java类文件里面DatagramSocket.receive是阻塞式.*/
		
		/*接收端*/
		package NormalFunction;
		
		import java.io.IOException;
		import java.net.DatagramPacket;
		import java.net.DatagramSocket;
		import java.net.InetAddress;
		
		public class TestReceived {
		
			/**
			 * @param args
			 * @throws IOException
			 */
			public static void main(String[] args) throws IOException {
				// TODO Auto-generated method stub
				/* 创建接收端套接字. */
				DatagramSocket rdgs = new DatagramSocket(15000);
				byte[] rbuffer = new byte[1024];
				int rlength = rbuffer.length;
				DatagramPacket rdgp = new DatagramPacket(rbuffer, rlength);
				rdgs.receive(rdgp);
				InetAddress raddress = rdgp.getAddress();
				String ipName = raddress.getHostName();/* 获取发送端主机名. */
				String ipX = raddress.getHostAddress(); /* 获取发送端IP地址. */
		
				/* 接收端解析并且显示在控制台. */
				byte[] Consolebuffer = rdgp.getData();
				int Consolelength = Consolebuffer.length;
				// String ConsoleString = new String(Consolebuffer);
				String ConsoleString = new String(Consolebuffer, 0, Consolelength);
				System.out.println("ReceivedFromName:" + ipName + "  ip:" + ipX
						+ "  Content:" + ConsoleString);
				rdgs.close();		
			}		
		}
		
		
		/*发送端*/
		package NormalFunction;
		
		import java.io.IOException;
		import java.net.DatagramPacket;
		import java.net.DatagramSocket;
		import java.net.InetAddress;
		
		public class TestSend {
		
			public static void main(String[] args) throws IOException {
		
				/* 创建套接字对象-发送端 */
				DatagramSocket dgs = new DatagramSocket();
				byte[] sendbys = "thisTestSenderData!".getBytes();
				int sendlength = sendbys.length;
				InetAddress address = InetAddress.getByName("192.168.0.168");
				int port = 15000;
				DatagramPacket senddgs = new DatagramPacket(sendbys, sendlength,
						address, port);
				dgs.send(senddgs);
				dgs.close();
			}
		}


		/*combine_files*/
		package NormalFunction;
		
		import java.io.IOException;
		import java.net.DatagramPacket;
		import java.net.DatagramSocket;
		import java.net.InetAddress;
		
		public class Sender_Receive {
		
			/**
			 * @param args
			 * @throws IOException
			 */
			public static void main(String[] args) throws IOException {
				// TODO Auto-generated method stub
				// TODO Auto-generated method stub
				/* 创建接收端套接字. */
				DatagramSocket rdgs = null;
				try {
					rdgs = new DatagramSocket(15000);
				} catch (Exception e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
				byte[] rbuffer = new byte[1024];
				int rlength = rbuffer.length;
				DatagramPacket rdgp = new DatagramPacket(rbuffer, rlength);
				try {
					rdgs.receive(rdgp);
				} catch (Exception e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
				InetAddress raddress = rdgp.getAddress();
				String ipName = raddress.getHostName();/* 获取发送端主机名. */
				String ipX = raddress.getHostAddress(); /* 获取发送端IP地址. */
		
				/* 接收端解析并且显示在控制台. */
				byte[] Consolebuffer = rdgp.getData();
				int Consolelength = Consolebuffer.length;
				// String ConsoleString = new String(Consolebuffer);
				String ConsoleString = new String(Consolebuffer, 0, Consolelength);
				System.out.println("ReceivedFromName:" + ipName + "  ip:" + ipX
						+ "  Content:" + ConsoleString);
				rdgs.close();
				System.out.println("this is a deviding line!");
		
				/* 创建套接字对象-发送端 */
				DatagramSocket dgs = new DatagramSocket();
				byte[] sendbys = "thisTestSenderData!".getBytes();
				int sendlength = sendbys.length;
				InetAddress address = InetAddress.getByName("192.168.0.168");
				int port = 15000;
				DatagramPacket senddgs = new DatagramPacket(sendbys, sendlength,
						address, port);
				dgs.send(senddgs);
				dgs.close();		
			}		
		}
		```
- 数据发送=>聊天窗口
 - 将上述的单条数据发送改为由键盘输入的发送，并且接收，用多线程实现，不使用多线程为上代码问题。。
 	- UDP:Code eg-runable实现： 
		```java
		/*实现runble接口实现方式.*/
		package NormalFunction;
		
		import java.io.BufferedReader;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.net.DatagramPacket;
		import java.net.DatagramSocket;
		import java.net.InetAddress;
		import java.net.SocketException;
		
		class sender implements Runnable {
			private DatagramSocket sd_sender;
		
			public sender(DatagramSocket sd_sender) {
				this.sd_sender = sd_sender;
			}
		
			@Override
			public void run() {
				try {
					/* 键盘获取数据. */
					BufferedReader br = new BufferedReader(new InputStreamReader(
							System.in));
					String line = null;
					while ((line = br.readLine()) != null) {
						if ("over!".equals(line)) {
							break;
						}
						/* 数据打包并且发送. */
						byte[] bys = line.getBytes();
						DatagramPacket dp_sender = new DatagramPacket(bys, bys.length,
								InetAddress.getByName("192.168.0.255"), 15360);
						sd_sender.send(dp_sender);
					}
					sd_sender.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
		
		class receiver implements Runnable {
			private DatagramSocket ds_receiver;
		
			public receiver(DatagramSocket ds_receiver) {
				this.ds_receiver = ds_receiver;
			}
		
			@Override
			public void run() {
				try {
					while (true) {
						byte[] bys = new byte[1024];
						DatagramPacket dp_receiver = new DatagramPacket(bys, bys.length);
						ds_receiver.receive(dp_receiver);
		
						/* parseData */
						String ip = dp_receiver.getAddress().getHostAddress();
						String s = new String(dp_receiver.getData(), 0,
								dp_receiver.getLength());
						System.out.println("from " + ip + " data is : " + s);
					}
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
		
		public class MultiThreadSend_Receive {
			public static void main(String[] args) throws SocketException {
				DatagramSocket ds_client = new DatagramSocket();
				DatagramSocket ds_receiver = new DatagramSocket(15360);
		
				sender sender_client = new sender(ds_client);
				receiver receiver_client = new receiver(ds_receiver);
		
				Thread thread_client = new Thread(sender_client);
				Thread thread_receive = new Thread(receiver_client);
				thread_client.start();
				thread_receive.start();
			}
		
		}
		```
	- UDP:Code eg-extends实现：
		```java
		package NormalFunction;
		
		import java.io.BufferedReader;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.net.DatagramPacket;
		import java.net.DatagramSocket;
		import java.net.InetAddress;
		import java.net.SocketException;
		
		import com.sun.webkit.ThemeClient;
		
		class sender extends Thread {
			private DatagramSocket sd_sender;
		
			public sender(DatagramSocket sd_sender) {
				this.sd_sender = sd_sender;
			}
		
			@Override
			public void run() {
				try {
					/* 键盘获取数据. */
					BufferedReader br = new BufferedReader(new InputStreamReader(
							System.in));
					String line = null;
					while ((line = br.readLine()) != null) {
						if ("over!".equals(line)) {
							break;
						}
						/* 数据打包并且发送至广播域. */
						byte[] bys = line.getBytes();
						DatagramPacket dp_sender = new DatagramPacket(bys, bys.length,
								InetAddress.getByName("192.168.0.255"), 15360);
						sd_sender.send(dp_sender);
					}
					sd_sender.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
		
		class receiver extends Thread {
			private DatagramSocket ds_receiver;
		
			public receiver(DatagramSocket ds_receiver) {
				this.ds_receiver = ds_receiver;
			}
		
			@Override
			public void run() {
				try {
					while (true) {
						byte[] bys = new byte[1024];
						DatagramPacket dp_receiver = new DatagramPacket(bys, bys.length);
						ds_receiver.receive(dp_receiver);
		
						/* parseData */
						String ip = dp_receiver.getAddress().getHostAddress();
						String s = new String(dp_receiver.getData(), 0,
								dp_receiver.getLength());
						System.out.println("from " + ip + " data is : " + s);
					}
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
		
		public class MultiThreadSend_Receive {
			public static void main(String[] args) throws SocketException {
				DatagramSocket ds_client = new DatagramSocket();
				DatagramSocket ds_receiver = new DatagramSocket(15360);
		
				sender sender_client = new sender(ds_client);
				receiver receiver_client = new receiver(ds_receiver);
		
				sender_client.start();
				receiver_client.start();
			}
		
		}
		```
- TCP实现的数据传送
**描述：**UDP数据包是非连接方式传播的，也就是说只保证自己的数据在网内广播，不是针对性的链接，同时也不保证数据被指定的机器接收到，TCP是连接式连接的，保证数据的传送，但是效率偏低，同样UDP也存在着一个问题就是容易产生垃圾广播数据，尤其是在非令牌环路广播域中，产生大量垃圾数据，造成网络速度迟缓。

	- TCP：Code实现单条数据的传输与发送.
		```java
		/*for example*/
		
		/*数据发送端*/
		package CompareTool;
		
		import java.io.IOException;
		import java.io.OutputStream;
		import java.net.Socket;
		import java.net.UnknownHostException;
		
		public class TCP_Client {
			public static void main(String[] args) throws UnknownHostException,
					IOException {		
				Socket clientSocket = new Socket("192.168.119.9", 15360);
				OutputStream clientStream = clientSocket.getOutputStream();
				clientStream.write("thisSenderData!".getBytes());
				clientSocket.close();
			}
		}
		
		/*数据服务器端*/
		package CompareTool;
		
		import java.io.IOException;
		import java.io.InputStream;
		import java.net.ServerSocket;
		import java.net.Socket;
		
		public class TCP_Server {
			public static void main(String[] args) throws IOException {
				ServerSocket serverSocket = new ServerSocket(15360);
				Socket nextSocket = serverSocket.accept();
				byte[] bysServer = new byte[1024];
				
				InputStream ServerStream = nextSocket.getInputStream();
				int serverLength = ServerStream.read(bysServer);
				String getString = new String(bysServer, 0, serverLength);
				System.out.println("ReceivedData:" + getString);
				serverSocket.close();
			}
		}
		
		/*服务器端实现方式二：*/
		package CompareTool;
		
		import java.io.BufferedReader;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.net.ServerSocket;
		import java.net.Socket;
		
		public class TCP_Server {
			public static void main(String[] args) throws IOException {
				ServerSocket serverSocket = new ServerSocket(15360);
				Socket nextSocket = serverSocket.accept();
		
				BufferedReader serverReader = new BufferedReader(new InputStreamReader(
						nextSocket.getInputStream()));
				String lineServer = null;
				while ((lineServer = serverReader.readLine()) != null) {
					System.out.println("ReceivedData:" + lineServer);
				}
				serverSocket.close();
			}
		}
		```

	- TCP：Code实现的由键盘输入实现和数据发送-控制台显示.

		```java
		/*客户端*/
		package ImproveTCP;
		
		import java.io.BufferedReader;
		import java.io.BufferedWriter;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.io.OutputStreamWriter;
		import java.net.Socket;
		
		public class ClientPort {
			public static void main(String[] args) throws IOException {
				/* 创建套接字对象 */
				Socket clientSocket = new Socket("192.168.119.9", 15360);
				
				/*输入输出流的定义.*/
				BufferedReader clientReader = new BufferedReader(new InputStreamReader(
						System.in));	 /*从键盘获取流，因此是reader.*/
				BufferedWriter clientWrite = new BufferedWriter(new OutputStreamWriter(
						clientSocket.getOutputStream()));	/*将套接字的数据流暴露出来，进行写入，发送。*/
				String clientString = null;
				
				/*每一行输出一次，遇到over!终止发送数据.*/
				while((clientString = clientReader.readLine())!= null){
					if ("over!".equals(clientString)) {
						break;
					}
					clientWrite.write(clientString);
					clientWrite.newLine();
					clientWrite.flush();
				}		
				clientSocket.close();
			}			
		}
		
		
		/*服务器端*/
		package ImproveTCP;
		
		import java.io.BufferedReader;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.net.ServerSocket;
		import java.net.Socket;
		
		public class ServerPort {
		
			public static void main(String[] args) throws IOException {
				ServerSocket serverSocket = new ServerSocket(15360);
		
				/* 收到数据需要提示发送端，当前已收到数据 */
				Socket nextSocket = serverSocket.accept();
				String serverString = null;
				BufferedReader serverReader = new BufferedReader(new InputStreamReader(
						nextSocket.getInputStream()));
				while ((serverString = serverReader.readLine()) != null) {
					System.out.println("ServerPort: current receivedData:" + serverString);
				}
				serverSocket.close();
			}
		}
		```
	- TCP：Code实现的由键盘输入实现和数据发送-数据写入到文本文档.
		```java
		/*client code*/
		package TCP_TXT;
		
		import java.io.BufferedReader;
		import java.io.BufferedWriter;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.io.OutputStreamWriter;
		import java.net.Socket;
		
		public class ClientFiles {
		
			public static void main(String[] args) throws IOException {
				// TODO Auto-generated method stub
				Socket clientSocket = new Socket("192.168.119.9", 15360);
				BufferedReader clientReader = new BufferedReader(new InputStreamReader(
						System.in));
				BufferedWriter clientWriter = new BufferedWriter(
						new OutputStreamWriter(clientSocket.getOutputStream()));
				String clientString = null;
				while ((clientString = clientReader.readLine()) != null) {
					clientWriter.write(clientString);
					clientWriter.newLine();
					clientWriter.flush();
				}
				clientSocket.close();
			}
		
		}
		
		
		
		/*server code*/
		package TCP_TXT;
		
		import java.io.BufferedReader;
		import java.io.BufferedWriter;
		import java.io.File;
		import java.io.FileWriter;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.net.ServerSocket;
		import java.net.Socket;
		
		public class ServerFiles {
			public static void main(String[] args) throws IOException {
				File files = new File("test.txt");
				BufferedWriter fileWriter = new BufferedWriter(new FileWriter(files));
				ServerSocket serverSocket = new ServerSocket(15360);
				Socket nextSocket = serverSocket.accept();
				BufferedReader serverReader = new BufferedReader(new InputStreamReader(
						nextSocket.getInputStream()));
		
				String serverString = null;
				while ((serverString = serverReader.readLine()) != null) {
					if (serverString.equals("over!")) {
						fileWriter.close();
						break;
					}
					fileWriter.write(serverString);
					fileWriter.newLine();
					fileWriter.flush();
				}
				serverSocket.close();
		
			}
		}
		```
- TCP：Code，服务器端接受由客户端文本文件发送的数据- 
	- 显示在控制台&写入到文本文件两种方式，尤其提示，服务器端一定要注意关闭流/flush二者必选其一，不然无法显示，但是逻辑没有警告也不报错，但是就是达不到显示or复制文件的效果。
	- Code如下：

		```java
		/*for example----客户端*/
		package TCP_TXT_TO_SERVER;
		
		import java.io.BufferedReader;
		import java.io.BufferedWriter;
		import java.io.FileReader;
		import java.io.IOException;
		import java.io.OutputStreamWriter;
		import java.net.Socket;
		
		public class ClientPort {
			public static void main(String[] args) throws IOException {
				/*socket对象*/
				Socket clientSocket = new Socket("192.168.119.9", 15360);
				
				/*文本文件流*/
				BufferedReader clientfiles = new BufferedReader(new FileReader(
						"F:\\Java\\TestCc\\SourceTxt.txt"));
				
				/*通道数据流 */
				BufferedWriter clientWriter = new BufferedWriter(
						new OutputStreamWriter(clientSocket.getOutputStream()));
				/*行内容的读取并且写入发送*/
				String clientString = null;
				while ((clientString = clientfiles.readLine()) != null) {
					clientWriter.write(clientString);
					clientWriter.newLine();
					clientWriter.flush();
				}
				/*通道流，文件流，套接字的关闭.*/
				clientfiles.close();
				clientWriter.close();
				clientSocket.close();
			}
		}
		
		
		/*for example----服务器端*/
		package TCP_TXT_TO_SERVER;
		
		import java.io.BufferedReader;
		import java.io.BufferedWriter;
		import java.io.FileWriter;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.net.ServerSocket;
		import java.net.Socket;
		
		public class ServerPort {
			public static void main(String[] args) throws IOException {
				/*服务套接字对象*/
				ServerSocket serverSocket = new ServerSocket(15360);
				
				/*解析获取套接字，准备解析。*/
				Socket nextSocket = serverSocket.accept();
				
				/*将套接字的通道流暴露出来，准备读取通道流，属于流的读取，相对于内存是写入。*/
				BufferedReader serverReader = new BufferedReader(new InputStreamReader(
						nextSocket.getInputStream()));
				
				/*文本流的创建， 准备写入到文本写出内存。*/
				BufferedWriter filesWriter = new BufferedWriter(new FileWriter(
						"ServerText.txt"));
				
				String serverString = null;
				/* this Console expression. */		
				 while ((serverString = serverReader.readLine()) != null) {
				  System.out.println(serverString); } 
				 
				/* this store to text files. */
				/*while ((serverString = serverReader.readLine()) != null) {
					filesWriter.write(serverString);
					filesWriter.newLine();
					filesWriter.flush();
				}*/
				filesWriter.close();
				serverReader.close();
				nextSocket.close();
				serverSocket.close();
			}
		}
		```
	- 带提示的TCP文本传输，完成后提示客户端且关闭连接-实现上个人认为比较完美。
		```java
		/*for example-服务器端*/
		package TCP_TXT_TO_SERVER;
		
		import java.io.BufferedReader;
		import java.io.BufferedWriter;
		import java.io.FileWriter;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.io.OutputStreamWriter;
		import java.net.ServerSocket;
		import java.net.Socket;
		
		public class ServerPort {
			public static void main(String[] args) throws IOException {
				/* 服务套接字对象 */
				ServerSocket serverSocket = new ServerSocket(15360);
		
				/* 解析获取套接字，准备解析。 */
				Socket nextSocket = serverSocket.accept();
		
				/* 将套接字的通道流暴露出来，准备读取通道流，属于流的读取，相对于内存是写入。 */
				BufferedReader serverReader = new BufferedReader(new InputStreamReader(
						nextSocket.getInputStream()));
		
				/* 文本流的创建， 准备写入到文本写出内存。 */
				BufferedWriter filesWriter = new BufferedWriter(new FileWriter(
						"ServerText.txt"));
		
				String serverString = null;
				while ((serverString = serverReader.readLine()) != null) {
					filesWriter.write(serverString);
					filesWriter.newLine();
					filesWriter.flush();
				}
				/* 给客户端发送提示：传输完成！ */
				BufferedWriter tipServer = new BufferedWriter(new OutputStreamWriter(
						nextSocket.getOutputStream()));
				tipServer.write("传输完成！");
				tipServer.newLine();	/*此行可略。*/
				tipServer.flush();	/*无此行，客户端收到提示为null.*/
				/nextSocket.shutdownOutput();  /*可略，因为后面有关闭.*/
		
				/* 释放对应资源。 */
				filesWriter.close();
				serverReader.close();
				nextSocket.close();
				serverSocket.close();
			}
		}
		
		
		/*for example-客户端*/
		package TCP_TXT_TO_SERVER;
		
		import java.io.BufferedReader;
		import java.io.BufferedWriter;
		import java.io.FileReader;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.io.OutputStreamWriter;
		import java.net.Socket;
		
		public class ClientPort {
			public static void main(String[] args) throws IOException {
				/* socket对象 */
				Socket clientSocket = new Socket("192.168.119.9", 15360);
		
				/* 文本文件流 */
				BufferedReader clientfiles = new BufferedReader(new FileReader(
						"F:\\Java\\TestCc\\SourceTxt.txt"));
		
				/* 通道数据流 */
				BufferedWriter clientWriter = new BufferedWriter(
						new OutputStreamWriter(clientSocket.getOutputStream()));
				/* 行内容的读取并且写入发送 */
				String clientString = null;
				while ((clientString = clientfiles.readLine()) != null) {
					clientWriter.write(clientString);
					clientWriter.newLine();/*此两行不影响显示效果.*/
					clientWriter.flush();
				}
				/* 通道流，文件流，套接字的关闭. */
				clientSocket.shutdownOutput();
		
				/* 接收服务器端的提示：传输完成！ */
				BufferedReader tipFromServer = new BufferedReader(
						new InputStreamReader(clientSocket.getInputStream()));
				String temp = tipFromServer.readLine();
				System.out.println("提示：" + temp);
		
				clientfiles.close();
				clientSocket.close();
			}
		}
		```
	- TCP-服务器端同时接收多个客户端的上传.

		```java
		/*for example*/
			- 要实现线程的多线程访问，需要做到的是，将服务器端主实现部分分割，客户端保留。同时主函数调用服务器线程，一旦客户端运行就可以捕捉到数据包，然后将文件存档，利用CurrentMillionTime可以实现名称问题，但是仍然存在一个同时访问情况下的高并发问题。
		
		/*客戶端*/
		import java.io.BufferedReader;
		import java.io.BufferedWriter;
		import java.io.FileReader;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.io.OutputStreamWriter;
		import java.net.Socket;
		
		public class UploadClient {
			public static void main(String[] args) throws IOException {
				
				Socket s = new Socket("192.168.119.9", 15360);	
				BufferedReader br = new BufferedReader(new FileReader(
						"TestAa.java"));
				
				BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(
						s.getOutputStream()));
		
				String line = null;
				while ((line = br.readLine()) != null) {
					bw.write(line);
					bw.newLine();
					bw.flush();
				}	
				s.shutdownOutput();
			
				BufferedReader brClient = new BufferedReader(new InputStreamReader(
						s.getInputStream()));
				String client = brClient.readLine();
				System.out.println(client);
			
				br.close();
				s.close();
			}
		}
		
		
		/*服务器线程*/
		import java.io.BufferedReader;
		import java.io.BufferedWriter;
		import java.io.FileWriter;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.io.OutputStreamWriter;
		import java.net.Socket;
		
		public class UserThread implements Runnable {
			private Socket s;
		
			public UserThread(Socket s) {
				this.s = s;
			}
		
			@Override
			public void run() {
				try {			
					BufferedReader br = new BufferedReader(new InputStreamReader(
							s.getInputStream()));			
		
					/*文件名問題*/
					String newName = System.currentTimeMillis() + ".java";
					BufferedWriter bw = new BufferedWriter(new FileWriter(newName));
		
					String line = null;
					while ((line = br.readLine()) != null) { // 阻塞
						bw.write(line);
						bw.newLine();
						bw.flush();
					}
		
					/*提示*/
					BufferedWriter bwServer = new BufferedWriter(
							new OutputStreamWriter(s.getOutputStream()));
					bwServer.write("Over!");		
					bwServer.flush();
					
					bw.close();
					s.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		
		}
		
		
		/*服务器主函数*/
		import java.io.IOException;
		import java.net.ServerSocket;
		import java.net.Socket;
		
		public class UploadServer {
			public static void main(String[] args) throws IOException {
				/*创建ServerSocket对象但是关闭ss.*/
				ServerSocket ss = new ServerSocket(11111);
				while (true) {
					Socket s = ss.accept();
					new Thread(new UserThread(s)).start();
				}
			}
		}		
		```
	总结：数据的传输需要
	>发送端-套接字，写入到内存-BufferedReader-Socket.getInputStreamReader匹配->写入到流，BufferedWriter->socket.getOutputStreamWriter...然后自动发送。
	>>服务器端则需要定义ServerSocket，accep函数获取Socket，然后将通道流暴露出来，然后利用BufferReader将流读取到内存，对应BufferedReader.readLine()，获取为String，然后可以直接显示在控制台，or创建BufferWriter->new BufferedWriter(new FileWriter("path"))->利用BufferedWriter对象，调用writer()函数，注意要显示效果，需要传输关闭流/flush即可。