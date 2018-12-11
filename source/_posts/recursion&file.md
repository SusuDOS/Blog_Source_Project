---
title: recursion&file
date: 2018-11-6 21:45:49
categories: Java基础学习
tags: [recursion, file字节流]
---
## recursion&file
**描述：**

递归做为一种算法在程序设计语言中广泛应用。 一个过程或函数在其定义或说明中有直接或间接调用自身的一种方法，它通常把一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解。递归策略只需少量的程序就可描述出解题过程所需要的多次重复计算，大大地减少了程序的代码量。递归的能力在于用有限的语句来定义对象的无限集合。一般来说，递归需要有边界条件、递归前进段和递归返回段。当边界条件不满足时，递归前进；当边界条件满足时，递归返回。

递归算法解题相对常用的算法如普通循环等，运行效率较低。因此，应该尽量避免使用递归，除非没有更好的算法或者某种特定情况，递归更为适合的时候。在递归调用的过程当中系统为每一层的返回点、局部量等开辟了栈来存储。递归次数过多容易造成栈溢出等。

***Java中使用递归注意：构造方法不能递归使用。***
### 递归代码实例：
- 斐波那契
```java
/*for example-Fibonacci*/
/*思考如何用递归实现斐波那契数列的存储和显示。*/
public static int fib(int n) {
		if (n == 1 || n == 2) {
			return 1;
		} else {
			return fib(n - 1) + fib(n - 2);
		}
	}

/*明显的优势是将有用的数据存储在数组里面。*/
arr[0] = 1;
arr[1] = 1;
for (int x = 2; x < arr.length; x++) {
	arr[x] = arr[x - 2] + arr[x - 1];			
}

/*不断替换a，b对应的数据，数据不保留。*/
int a = 1;
int b = 1;
for (int x = 0; x < 18; x++) {
	// 每运行一次将a,b对应的数据刷新一遍。
	int temp = a;
	a = b;
	b = temp + b;
	System.out.println(a);
}
```

## IO流
描述：

按照流的方式进行输入输出，数据被当成无结构的字节序或字符序列。从流中取得数据的操作称为提取操作，而向流中添加数据的操作称为插入操作。用来进行输入输出操作的流就称为IO流...
### IO流分类：
- 流向
	可以这样理解，流的输入还是输出性是相对于内存来描述的。相对于内存是进入内存，则为输入流--读取数据；相对于内存是流出内存，则是输出流--写出数据--写入到外部存储设备。
- 数据类型
		字节流：字节输入流，字节输出流。
		字符流：字符输入流，字符输出流。

*Tip：建议使用字节流进行文件的输入和输出。*

### 字节流FileInput/OutStream.

- FileOutputStream写出数据

	使用中，主函数必须public static void main(String[] args) throws IOException/try...catch...

	```java
	/*for example*/
			FileOutputStream fos = new FileOutputStream("fos.txt");			
			fos.write("hello,world.".getBytes());			
			fos.close();
	```

- FileInputStream读取数据

	使用中，主函数必须public static void main(String[] args) throws IOException/try...catch...

	```
	/*for example*/
			FileInputStream fis = new FileInputStream("fos.txt");
			//way_01:	
			int by = 0;
			while((by=fis.read())!=-1) {
				System.out.print((char)by);
			}

			//way_02
			char by = 0;		
			while ((by = (char) fis.read()) != -1) {
				System.out.print(by);
			}
		
			//way_03
				byte[] bys = new byte[1024];
				int len = 0;
				while((len=fis.read(bys))!=-1) {
					System.out.print(new String(bys,0,len));
				}		
			fos.close();
	```

- 字节缓冲区流
	- BufferedOutputStream.
	- BufferedInputStream.

	![](https://i.imgur.com/z8fV5Ov.png)

	```
		//字符缓冲区的使用方法.
		public static void Way_One(String srcString, String destString)
				throws IOException {
			BufferedInputStream bis = new BufferedInputStream(new FileInputStream(
					srcString));
			BufferedOutputStream bos = new BufferedOutputStream(
					new FileOutputStream(destString));
	
			byte[] bys = new byte[1024];
			int len = 0;
			while ((len = bis.read(bys)) != -1) {
				bos.write(bys, 0, len);
			}
	
			bos.close();
			bis.close();
		}
	
		
		public static void Way_Two(String srcString, String destString)
				throws IOException {
			BufferedInputStream bis = new BufferedInputStream(new FileInputStream(
					srcString));
			BufferedOutputStream bos = new BufferedOutputStream(
					new FileOutputStream(destString));
	
			int by = 0;
			while ((by = bis.read()) != -1) {
				bos.write(by);
	
			}

		fos.close();
		fis.close();
	
	```
- 字符流	
	- Reader
		```
			FileReader
			BufferedReader
		```
	- Writer
		```
			FileWriter
			BufferedWriter
		```