---
title: CommonMark-TreeSet&Set&cCollection
date: 2018-11-2 22:45:49
categories: Java基础学习
tags: [Java, Colletion, Set]
---

## 注册&登录
- 首先需要设置登录界面，进入界面后选择想进行的操作。
- 注册界面进入后，提示用户设置注册账户&用户密码，然后将数据追加到数据库--集合，此处不考虑用户存在问题，因为程序只运行一次，注册仅有一个账户。
- 登录界面，提示输入账户&密码，获取得到数据后遍历集合判断是否存在，存在则判断登录成功。
- 登录成功后可以进行其他操作，操作完成后提示是否需要退出or继续操作。
- 代码实例如下：
	```java
	package TestAa;
	
	import java.util.ArrayList;
	import java.util.Scanner;
	
	/*用户类，描述用户的相关信息.*/
	class user {
		private String name;
		private String password;
	
		public String getName() {
			return name;
		}
	
		public void setName(String name) {
			this.name = name;
		}
	
		public String getPassword() {
			return password;
		}
	
		public void setPassword(String password) {
			this.password = password;
		}
	
		@Override
		public int hashCode() {
			final int prime = 31;
			int result = 1;
			result = prime * result + ((name == null) ? 0 : name.hashCode());
			result = prime * result
					+ ((password == null) ? 0 : password.hashCode());
			return result;
		}
	
		@Override
		public boolean equals(Object obj) {
			if (this == obj)
				return true;
			if (obj == null)
				return false;
			if (getClass() != obj.getClass())
				return false;
			user other = (user) obj;
			if (name == null) {
				if (other.name != null)
					return false;
			} else if (!name.equals(other.name))
				return false;
			if (password == null) {
				if (other.password != null)
					return false;
			} else if (!password.equals(other.password))
				return false;
			return true;
		}
	
		public user() {
		}
	
		public user(String name, String password) {
			this.name = name;
			this.password = password;
		}
	
	}
	
	/* 操作类，个人认为此处没有用实现接口类的必要，完全定义一个对数据库的操作类即可. */
	class OperatorClass {
		// 静态修饰，随着类的加载而加载，被所有的成员所共用。
		private static ArrayList<user> array = new ArrayList<user>();
	
		// 判断登录成功与否操作.
		public boolean isLogin(String username, String password) {
			boolean flag = false;
	
			for (user u : array) {
				if (u.getName().equals(username)
						&& u.getPassword().equals(password)) {
					flag = true;
					break;
				}
			}
			return flag;
		}
	
		// 注册操作，添加至集合.
		public void regist(user user) {
			array.add(user);
		}
	}
	
	class application {
		public static void start() {
			System.out.println("游戏开始了.");
		}
	
		public static void end() {
			System.out.println("游戏结束了.");
		}
	
		public application() {
			super();
		}
	}
	
	/* 主类，对效果进行操作，也称之为测试类. */
	public class regist_Log {
	
		/**
		 * @param args
		 */
		public static void main(String[] args) {
			// TODO Auto-generated method stub
			// 限制循环次数，不使用while是确定正常用户操作的特性.
			for (int Cast = 0; Cast < 30; ++Cast) {
				System.out.println("--------------------用户主界面--------------------");
				System.out.println("1:\t用户注册");
				System.out.println("2:\t用户登录");
				System.out.println("3:\t用户退出");
				Scanner sc = new Scanner(System.in);
				System.out.println("请选择你想进行的操作：");
				String lineAa = sc.nextLine();
				OperatorClass Operator = new OperatorClass();
	
				// 登录主界面
				switch (lineAa) {
				case "1":
					// 注册界面
					System.out.println("----------------注册界面----------------");
					System.out.println("请输入用户名：");
					String userName = sc.nextLine();
					System.out.println("请输入密码：");
					String passWord = sc.nextLine();
					user user = new user(userName, passWord);
					Operator.regist(user);
					System.out.println("注册成功");
					break;
				case "2":
					// 登录界面，请输入用户名和密码
					for (int count = 0; count < 3;) {
						System.out.println("----------------登录界面----------------");
						System.out.println("请输入用户名：");
						String username = sc.nextLine();
						System.out.println("请输入密码：");
						String password = sc.nextLine();
	
						boolean flag = Operator.isLogin(username, password);
	
						if (flag) {
							System.out.println("登录成功,可以开始操作了.");
							System.out.println("继续么？y/n");
							while (true) {
								String continueAa = sc.nextLine();
								if (continueAa.equalsIgnoreCase("y")) {
									application.start();
									System.out.println("你还玩吗?y/n");
								} else {
									break;
								}
							}
							System.out.println("欢迎下次再来.");
							System.exit(0);
						} else {
							System.out.println("登录失败.");
							++count;
						}
						if (count == 3)
							System.exit(0);
					}
					break;
				case "3":
					System.out.println("谢谢使用，欢迎下次再来");
					System.exit(0);
				default:
					break;
				}
			}
		}
	}
	
	```


## Set集合
描述：是一个Collection的子接口，对比于Collection子接口List。前者为不重复元素的集合，后者为可以重复元素的集合。

- ![](https://i.imgur.com/Uo98zQX.png)
- Set集合的特点：	
	>无序&唯一。
- HashSet集合：
描述：底层数据结构是哈希表-链表数组。要实现数据的唯一性，其使用：hashCode()和equals()两个方法。
执行顺序：
	```
	if hashcode相同
		if 内容相同
			元素存在
		else
			追加到集合
	else
		追加到集合.
	```
- 关于hashcode&equals使用自动生成即可。
- 代码实例
	- 实例一：HashSet存储字符串并遍历
		```java
		public static void main(String[] args) {		
			Set<String> set = new HashSet<String>();		
			set.add("hello");
			set.add("Jack");
			set.add("world");
			set.add("Jack");
			set.add("world");
			// 增强for
			for (String s : set) {
				System.out.println(s);
			}
		}
		```

	- 实例二：HashSet存储自定义对象并遍历

		```java
		package TestAa;
		
		import java.util.HashSet;
		import java.util.Set;
		
		class animal {
			private String name;
			private int age;
		
			public animal() {
				super();
				// TODO Auto-generated constructor stub
			}
		
			public animal(String name, int age) {
				super();
				this.name = name;
				this.age = age;
			}
		
			public String getName() {
				return name;
			}
		
			public void setName(String name) {
				this.name = name;
			}
		
			public int getAge() {
				return age;
			}
		
			public void setAge(int age) {
				this.age = age;
			}
		
			@Override
			public int hashCode() {
				final int prime = 31;
				int result = 1;
				result = prime * result + age;
				result = prime * result + ((name == null) ? 0 : name.hashCode());
				return result;
			}
		
			@Override
			public boolean equals(Object obj) {
				if (this == obj)
					return true;
				if (obj == null)
					return false;
				if (getClass() != obj.getClass())
					return false;
				animal other = (animal) obj;
				if (age != other.age)
					return false;
				if (name == null) {
					if (other.name != null)
						return false;
				} else if (!name.equals(other.name))
					return false;
				return true;
			}
		
			@Override
			public String toString() {
				return "animal [name=" + name + ", age=" + age + "]";
			}
		}
		
		public class CompareBb {
			/**
			 * @param args
			 */
			public static void main(String[] args) {
				// TODO Auto-generated method stub
		
				Set<animal> animalSet = new HashSet<animal>();
				animalSet.add(new animal("Tom", 5));
				animalSet.add(new animal("Tom", 5));
		
				animalSet.add(new animal("Jack", 5));
				animalSet.add(new animal("Jack", 4));
		
				animalSet.add(new animal("Rose", 5));
				animalSet.add(new animal("Rose", 3));
				System.out.println("animalSet:" + animalSet);
				for (animal temp : animalSet) {
					System.out.println("name:" + temp.getName() + "\tage:"
							+ temp.getAge());
				}
			}
		}
		
		```
- LinkedHashSet
	- LinkedHashSet:底层数据结构由哈希表和链表组成。
	- 哈希表保证元素的唯一性。
	- 链表保证元素有素,由于数据的存储在底部一定是按照一定的顺序记录的，一般指数据的有序性指的是输出输入顺序不会发生改变。
	- 代码实例：

		``` 
		import java.util.LinkedHashSet;
		public class LinkedHashSetCc {
			public static void main(String[] args) {		
				LinkedHashSet<String> hs = new LinkedHashSet<String>();
				
				hs.add("hello");
				hs.add("world");
				hs.add("Tom");
				hs.add("world");
				
				for (String s : hs) {
					System.out.println(s);
				}
			}
		}
		```

- TreeSet
	- 底层数据结构是红黑树-是一个自平衡的二叉树-数据结构中描述为二叉排序树。
	- 元素排序实现
		- 自然排序：元素本身可以比较，让元素所属的类实现Comparable接口.

			```java
			/*for example*/
			public static void main(String[] args) {
				// 创建集合对象
				// 自然顺序进行排序
				TreeSet<Integer> ts = new TreeSet<Integer>();		
				ts.add(20);
				ts.add(18);
				ts.add(22);	
				ts.add(19);	
				ts.add(24);		
				for (Integer i : ts) {
					System.out.println(i);
				}
			}
			```
		- 排序：集合具备比较性，让集合构造方法接收Comparator的实现类对象。				
			```java
			/*此为也为Compareable的实现方式.*/
			package TestAa;
			
			import java.util.TreeSet;
			
			/*如果一个类的元素要想能够进行自然排序，就必须实现自然排序接口Comparable*/
			
			class animalCc implements Comparable<animalCc> {
				private String name;
				private int age;
			
				public animalCc() {
				}
			
				public animalCc(String name, int age) {
			
					this.name = name;
					this.age = age;
				}
			
				public String getName() {
					return name;
				}
			
				public void setName(String name) {
					this.name = name;
				}
			
				public int getAge() {
					return age;
				}
			
				public void setAge(int age) {
					this.age = age;
				}
			
				@Override
				/* 关键性代码块. */
				public int compareTo(animalCc arg0) {
					// 规则为树的插入排序，小走左边，大走右边，相同则不添加。
					// return -1;添加顺序的逆序排
					// return 1;添加顺序的顺序排.
					// return 0; 自动生成的默认参数为0，其意思是与比较元素完全相同，所以元素只存储一个
					/*
					 * 设置排序规则：如：第一规则为年龄，，升序，其余自己分析，保证数据不丢失。
					 */
					/* 判断，年龄。 */
					int num1 = this.age - arg0.age;
					/* 判断姓名 */
					int num2 = num1 == 0 ? this.name.compareTo(arg0.name) : num1;
					return num2;
				}
			}
			
			public class TreeObject {
				public static void main(String[] args) {
					TreeSet<animalCc> ts = new TreeSet<animalCc>();
					animalCc s1 = new animalCc("Tom", 27);
					animalCc s2 = new animalCc("Jack", 29);
					animalCc s3 = new animalCc("Jack", 23);
					animalCc s4 = new animalCc("Coffee", 27);
					animalCc s5 = new animalCc("Tom", 22);
					animalCc s6 = new animalCc("Jack", 29);
					animalCc s7 = new animalCc("Tom", 22);
					ts.add(s1);
					ts.add(s2);
					ts.add(s3);
					ts.add(s4);
					ts.add(s5);
					ts.add(s6);
					ts.add(s7);
			
					for (animalCc s : ts) {
						System.out.println(s.getName() + "---" + s.getAge());
			
					}
				}
			}
		```
		- 比较器排序：代码.
		```java
		/*集合具备比较性,	让集合的构造方法接收一个比较器接口的子类对象 Comparator
		/*for example*/
		class student{
			成员变量
			成员方法
			构造方法
			标准代码块部分.
		}
		/*构造一个比较器接口的子类对象.*/
		import java.util.Comparator;
		public class MyComparator implements Comparator<Student> {
			
			public int compare(Student s1, Student s2) {
				// int num = this.name.length() - s.name.length();
				// this -- s1
				// s -- s2
		
				// 姓名长度
				int num = s1.getName().length() - s2.getName().length();
				// 姓名内容
				int num2 = num == 0 ? s1.getName().compareTo(s2.getName()) : num;
				// 年龄
				int num3 = num2 == 0 ? s1.getAge() - s2.getAge() : num2;
				return num3;
			}
		
		}
		
		/*测试组部分*/
		public class TreeSetDemo {
			public static void main(String[] args) {
				
				/*匿名内部类对其实现，无需要新建一个实现比较器接口的子类。-上部分也可以直接省略.*/
				TreeSet<Student> ts = new TreeSet<Student>(new Comparator<Student>() {
					@Override
					public int compare(Student s1, Student s2) {
						// 姓名长度
						int num = s1.getName().length() - s2.getName().length();
						// 姓名内容
						int num2 = num == 0 ? s1.getName().compareTo(s2.getName())
								: num;
						// 年龄
						int num3 = num2 == 0 ? s1.getAge() - s2.getAge() : num2;
						return num3;
					}
				});
		
				// 创建元素
				Student s1 = new Student("linqingxia", 27);
				Student s2 = new Student("zhangguorong", 29);
				Student s3 = new Student("wanglihong", 23);
				Student s4 = new Student("linqingxia", 27);
				Student s5 = new Student("liushishi", 22);
				Student s6 = new Student("wuqilong", 40);
				Student s7 = new Student("fengqingy", 22);
				Student s8 = new Student("linqingxia", 29);
		
				// 添加元素
				ts.add(s1);
				ts.add(s2);
				ts.add(s3);
				ts.add(s4);
				ts.add(s5);
				ts.add(s6);
				ts.add(s7);
				ts.add(s8);
		
				// 遍历
				for (Student s : ts) {
					System.out.println(s.getName() + "---" + s.getAge());
				}
			}
		}
	```
## Collection集合总结

### Collection子类结构
![](https://i.imgur.com/YeeWkSJ.png)

### Collection集合使用
- 集合使用的选择规则.
```java
if Uniqueness
	Set
		if Sort
			TreeSet
		else	
			HashSet
		Default HashSet.
		
else
	List
		if Safety
			Vector
		esle
			ArrayList or LinkedList
				Inquire：ArrayList
				add/del：LinkedList
		Default ArrayList。

Default ArrayList.

if Collection
	ArrayList.
```

- 数据结构选择

![](https://i.imgur.com/0I7xwSL.png)