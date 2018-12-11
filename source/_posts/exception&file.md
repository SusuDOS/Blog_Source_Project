---
title: exception&file
date: 2018-11-7 22:45:49
categories: Java基础学习
tags: [file exception, exception]
---

## exception
描述：程序非正常的编译&运行情况，实际使用过程中可能会出现非正常使用或者是代码逻辑上的不严谨导致的额出错。异常的处理规则是，原则上是本级处理，但是无处理语句会见exception抛到上级，直到处理。最高级别是JVM，则抛出error。
### 异常知识
- 异常的分类

	![](https://i.imgur.com/NBUUfEg.png)
- 异常的体系

	![](https://i.imgur.com/zqOVp3H.png)
- 异常的处理机制

	![](https://i.imgur.com/E6MpzFw.png)


- exception与runexception
	- 由分类的层次结构图可以得知，exception是一个大类，包括了runtimexcption.
	- 编译期间的错误是基本的语法错误，是必要进行处理的，是保证程序运行的前提。否则编译无法通过...
	- 运行期异常可以不处理，也可以处理。处理方式一般是抛给上一级，或者使用throw new exception().
	- 在代码块中经常会使用到throw与throws，它们的区别是一个前置是运行期间可能的错误，是可以不处理的，如:

	```java
	public static int divide(int a, int b){
		retuen a/b;	//此处考虑到b考虑为0，则需要throw异常。
	}
	
	public static int divide(int a, int b){
		if(b!=0)
			retuen a/b;	
		else
			throw new ArithmeticException();
	}
	```
				
- throw存在于一个函数，抛出的是一个异常对象，“throw new ArithmeticException();”说明这里有一个异常产生了，而throws可以抛出多个异常，说明的是有可能出错，跟的是类名。
- 异常处理格式&顺序
```java
try...catch...finally	//完全的标准异常处理格式.
try...catch...
try...catch...catch...
try...catch...catch...fianlly			
try...finally
```
- 代码实例
	- try...catch...作为一个完整的格式，一旦catch则异常处理完成，不继续执行相关的部分。
		```java
		/**for example/
		eg:
		try{
				...
			}catch(异常类名 变量名) {
				...
				e.printStackTrace();//显示简约的异常信息.
			}
		int a = 5, b = 0;
			try {
				System.out.println(a / b);
			} catch (ArithmeticException ae) {
				System.out.println("除数不能为0");
			}
			System.out.println("over");// 可以显示出over.
		
		
			eg:
			try{
					...
				}catch(异常类名 变量名) {
					...
				}
				catch(异常类名 变量名) {
					...
				}
					int a = 10;
					int b = 0;
					int[] arr = { 1, 2, 3 };
					try {
						System.out.println(a / b);
						System.out.println(arr[3]);
						System.out.println("testOne");
					} catch (ArithmeticException e) {
						System.out.println("testTwo");
					} catch (ArrayIndexOutOfBoundsException e) {
						System.out.println("testThree");
					} catch (Exception e) {
						System.out.println("testFour");
					}
					System.out.println("the last.");
					System.out.println("over.");
			/*执行结果：
			testTwo
			the last.
			over.
			*/
		
		
		eg:
		try{
				...
				
			}catch(异常类名 |异常类名 变量名) {
					...
			}
		
		try{
			System.out.println(a / b);
			System.out.println(arr[3]);
			} catch (ArithmeticException | ArrayIndexOutOfBoundsException e) {
				System.out.println("testAa");
				e.printStackTrace();//显示简约的异常信息.
			}
		
				System.out.println("over.");
		/*执行结果：
		testAa
		over.
		*/
		```
- finally异常处理
	- 自定义异常
		继承自Exception或者RuntimeException,只需要提供无参构造和一个带参构造即可。
		```
		/*自定义异常抛出*/
		package TestMc;
		import java.util.Scanner;
		class AaException extends Exception {
			public AaException() {
			}
		
			public AaException(String Message) {
				super(Message);
			}
		}
		class AaCheck {
			public void check(int a) throws AaException {
				if (a < 0 || a > 5) {
					throw new AaException();
				} else
					System.out.println("输入结果无误.");
			}
		}
		class TestCc {
			public static void main(String[] args) {
				System.out.println("Please input your grades:");
				Scanner Cv = new Scanner(System.in);
				int Bb = Cv.nextInt();
				AaCheck Ba = new AaCheck();
				try {
					Ba.check(Bb);
				} catch (Exception e) {
					e.printStackTrace();
				}
				System.out.println("Output Over.");
			}
		}
		
		```
- 继承异常的care
继承的子类异常限定范围应该比父类更加小，不能比父类的异常范围更大.
如：
	>父的方法有异常抛出,子的重写方法在抛出异常的时候必须要小于等于父的异常。
	>>父的方法没有异常抛出,子的重写方法不能有异常抛出。
	>>>父的方法抛出多个异常,子的重写方法必须比父少或者小。

## File
Java中对I/O操作的工具类，是用来操作文件的主要类。

文件和目录路径名的抽象表示：
用户界面和操作系统使用依赖于系统的路径名字符串命名文件和目录。 这个类提供了一个抽象的，独立于系统的层次化路径名的视图。 
- 构造方法
	- 该类的构造方法有四个，得到的都是对象类型数据，具体的使用如下：
	
		```
		File(String pathname)	根据一个路径得到File对象。
		File(String parent, String child)	根据一个目录和一个子文件/目录得到File对象。
		File(File parent, String child)	根据一个父File对象和一个子文件/目录得到File对象。
	
		/*for example*/
		File file = new File("e:\\demo\\a.txt");
		File file = new File("e:\\demo","a.txt");
		File file = new File("e:\\demo");
				  File file2 = new File(file,"a.txt");	
	
		/*第一行==第二行==第三+第四行.*/	
	```
- File类文件的操作 
	- 创建功能

		```
		/* 创建成功返回的true，否则false.*/
		public boolean createNewFile()	创建文件 如果存在这样的文件，就不创建了.
		
		/*以下两个只能创建文件夹，不能创建文件，就算是public boolean mkdir(“aa\\aa.txt”)也是创建的是aa.txt的文件夹.*/
		public boolean mkdir()	创建文件夹 如果存在这样的文件夹，就不创建了.
		public boolean mkdirs()	创建文件夹,如果父文件夹不存在，会帮你创建出来.
		
		/*for example*/
		
		File file = new File("e:\\demo"); //在e盘目录下创建一个文件夹demo.
		System.out.println("mkdir:" + file.mkdir());
		
		File file = new File("e:\\demos\\aa"); //在e盘目录下创建一个文件夹demos\aa.
		System.out.println("mkdir:" + file.mkdirs());
		
		File file = new File("e:\\demoss\\aa"); //在e盘目录下创建一个文件夹demoss\aa.
		System.out.println("mkdir:" + file.mkdir()); //无demoss创建失败.
				
		File file2 = new File("e:\\demo\\a.txt");//在e盘目录demo下创建一个文件a.txt
		System.out.println("createNewFile:" + file2.createNewFile());
		```
	- 删除功能

		```
		 /*删除功能:public boolean delete()*/
		
			如果你创建文件或者文件夹忘了写盘符路径，那么，默认在项目路径下。
		
			Java中的删除直接从硬盘删除，不删除到回收站。
		
			要删除一个文件夹，请注意该文件夹内不能包含文件或者文件夹，含有内容则无法删除。
		File file = new File("aaa\\bbb");
		File file2= new File("aaa");
				System.out.println("delete:" + file6.delete());
				System.out.println("delete:" + file7.delete());
		```

	- 重命名功能
		```
		public boolean renameTo(File dest)
		如果路径名相同，就是重命名，否则就是重命名&&剪切。 
		路径以盘符开始：绝对路径	如：c:\\a.txt
		路径不以盘符开始：相对路径	如：a.txt，默认本位置为项目根目录。
		
		/*for example*/
		File file2 = new File("E:\\testCc.txt");
		File newFile2 = new File("C:\\testCa.jpg");
		System.out.println("renameTo:" + file2.renameTo(newFile2));
		```

	- 判断功能
		```
		/*for example*/
		
		public boolean isDirectory()	判断是否是目录
		public boolean isFile()		判断是否是文件
		public boolean exists()		判断是否存在
		public boolean canRead()	判断是否可读
		public boolean canWrite()	判断是否可写
		public boolean isHidden()	判断是否隐藏

		public class FileTest {
			public static void main(String[] args) {
				
				File file = new File("testCc.txt");
		
				System.out.println("isDirectory:" + file.isDirectory());// false
				System.out.println("isFile:" + file.isFile());// true
				System.out.println("exists:" + file.exists());// true
				System.out.println("canRead:" + file.canRead());// true
				System.out.println("canWrite:" + file.canWrite());// true
				System.out.println("isHidden:" + file.isHidden());// false
			}
		}
		```
	- 获取功能
		```
		/*for example*/
		public String getAbsolutePath()		获取绝对路径
		public String getPath()		获取相对路径
		public String getName()		获取名称
		public long length()		获取长度。字节数
		public long lastModified()	获取最后一次的修改时间，毫秒值，初试时间0为1970-01-01:00:00。
		
		public class FileDemo {
			public static void main(String[] args) {
				// 创建文件对象
				File file = new File("demo\\test.txt");
		
				System.out.println("getAbsolutePath:" + file.getAbsolutePath());
				System.out.println("getPath:" + file.getPath());
				System.out.println("getName:" + file.getName());
				System.out.println("length:" + file.length());
				System.out.println("lastModified:" + file.lastModified());
				
				Date d = new Date();
				SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
				String s = sdf.format(d);
				System.out.println(s);
			}
		}
		```
	- 获取功能
		```
		public String[] list()	获取指定目录下的所有文件&&文件夹的名称，返回一个对应的String数组。
		public File[] listFiles()	获取指定目录下的所有文件&&文件夹的File对象数组。
		
		/*for example*/
		
		/*获取对应文件夹下的文件名称.*/
				File file = new File("C:\\");
		
				//public String[] list()
				String[] strArray = file.list();
				for (String s : strArray) {
					System.out.println(s);
				}
				System.out.println("----分----割----线----");
		
				//public File[] listFiles()+getName()返回名字.
				File[] fileArray = file.listFiles();
				for (File f : fileArray) {
					System.out.println(f.getName());
				}
		```
	- 过滤功能
		>描述：获取指定格式文件名字，并且输出。
		>>接口：文件名称过滤器:FilenameFilter借口。
		>>>函数如下：		
		```
		public String[] list(FilenameFilter filter)
		public File[] listFiles(FilenameFilter filter)
		```

		>>			
		
			/*获取ttf扩展名的文件*/--"C:\\Windows\\Boot\\Fonts"为测试文件路径.
			
			/*for example----普通for循环结构实现.*/
			
			import java.io.File;
			public class FileTest {
				public static void main(String[] args) {
					
					File file = new File(”C:\\Windows\\Boot\\Fonts“);	
					File[] fileArray = file.listFiles();
				
					for (File f : fileArray) {			
						if (f.isFile()) {			
							if (f.getName().endsWith(".ttf")) {			
								System.out.println(f.getName());
							}
						}
					}
				}
			}						
			
			/*for example----文件过滤器实现*/
			import java.io.File;
			import java.io.FilenameFilter;
			
			public class FileTest {
				public static void main(String[] args) {
					
					File file = new File("C:\\Windows\\Boot\\Fonts");
			
					String[] strArray = file.list(new FilenameFilter() {
						/*匿名类的使用。*/
						@Override
						public boolean accept(File dir, String name) {
			
							// return false，表示不添加;
							// return true,表示添加;
							
							return new File(dir, name).isFile() && name.endsWith(".ttf");
						}
					});
			
					// 遍历
					for (String s : strArray) {
						System.out.println(s);
					}
				}
			}
- 批量重命名
	- 源文件：
	- 目标文件：
	- 
	![](https://i.imgur.com/nhML0s4.png)

	>>
	```
	import java.io.File;
	
	public class FileDemo {
		public static void main(String[] args) {
			
			File srcFolder = new File("E:\\TestCc");
		
			File[] fileArray = srcFolder.listFiles();
		
			for (File file : fileArray) {		
	
				int index = name.indexOf("_");
				String numberString = name.substring(index + 1, index + 4);		
				int endIndex = name.lastIndexOf('_');
				String nameString = name.substring(endIndex);
				String newName = numberString.concat(nameString);
				File newFile = new File(srcFolder, newName);		
				file.renameTo(newFile);
			}
		}
	}
	
	```