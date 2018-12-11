---
title: Common-List&泛型
date: 2018-10-28 21:45:49
categories: Java基础学习
tags: [List, 泛型]
---
# List
## list子类及其特性
### ArrayList:
- 底层数据结构是数组，查询快，增删慢，线程不安全，效率高。
```java
/*for example*/
public static void main(String[] args) {		
	ArrayList array = new ArrayList();
	
	array.add("hello");
	array.add("world");
	
	Rem：1
	// array.add(new Integer(100));// 标准写法
	//array.add(10); // JDK5以后的自动装箱,等价于：array.add(Integer.valueOf(10));
	
	Iterator it = array.iterator();
	while (it.hasNext()) {
	Rem：1，若使用Rem处代码，强转数据类型：
		// String s = (String) it.next();			
		则：
		// ClassCastException
		
		String s = (String) it.next();
		System.out.println(s);
		/*代码黄色警告线，因为事先不指定的数据类型，若添加的数据类型不一致，强转后抛出ClassCaseExcpetion异常，为隐患使用方法.*/
	}
```

### Vector:
**特性：**底层数据结构是数组，查询快，增删慢，线程安全，效率低。
由于集合是JDK1.2后才有的特性，所以针对出现在JKD1.0的Vector本身有自己的添加获取功能，但是可以使用父类ArrayList对应功能所代替。
- 常用函数&方法
```java
/*1.0版本的本身独有功能：*/
添加：public void addElement(E obj)		--	add()
获取：public E elementAt(int index)		--	get()
	public Enumeration<E> elements()	--  iterator()
```

- Vector:代码实例	
```java
/*for example*/
public static void main(String[] args) {		
	Vector v = new Vector();

	v.addElement("hello");
	v.addElement("world");		
	
	for (int x = 0; x < v.size(); x++) {
		String s = (String) v.elementAt(x);
		System.out.println(s);
	}
	System.out.println("------------------");

	Enumeration en = v.elements(); 
	while (en.hasMoreElements()) {
		String s = (String) en.nextElement();
		System.out.println(s);
	}
}
```
		
### LinkedList:
**描述：**底层数据结构是链表，查询慢，增删快，线程不安全，效率高。由于其有不同的底层结构所有也有特有函数。
- LinkedList常用函数

```java	

添加：
	public void addFirst()	//表头添加，头插法.
	public void addLast()	//表尾添加，尾插法.
删除
	public Object removeFirst()	//删除第一个节点.
	public Object removeLast()	//删除第二个节点.
获取
	public Object getFirst()	//获取第一个节点.
	public Object getLast()		//获取最后一个节点.
```
- LinkedList:代码实例

```java
/*for example*/
public static void main(String[] args) {
	LinkedList link = new LinkedList();
	
	link.add("hello");	//后面追加.
	link.add("world");
	
	// public void addFirst(Object e)		
	// public void addLast(Object e)		

	// public Object getFirst()
	// System.out.println("getFirst:" + link.getFirst());
	// public Obejct getLast()
	// System.out.println("getLast:" + link.getLast());

	// public Object removeFirst()
	System.out.println("removeFirst:" + link.removeFirst());
	// public Object removeLast()
	System.out.println("removeLast:" + link.removeLast());

	// 输出对象名，tostring(),返回对象内容。
	System.out.println("link:" + link);
}
```
## 集合总结
- 代码：遍历字符串集合，仅保留重复元素的其中一个.

```java
public static void main(String[] args) {
	// TODO Auto-generated method stub
	ArrayList arrayStr = new ArrayList();
	arrayStr.add("Good");
	arrayStr.add("luck");
	arrayStr.add("tom");
	arrayStr.add("Good");
	arrayStr.add("Good");
	arrayStr.add("Good");
	arrayStr.add("luck");
	arrayStr.add("tom");
	Iterator It = arrayStr.iterator();
	ArrayList NewArrayStr = new ArrayList();
	/* Method:the first */
	while (It.hasNext()) {
		String temp = (String) It.next();
		if (!(NewArrayStr.contains(temp))) {
			NewArrayStr.add(temp);
		}
	}
	System.out.println("Result:" + NewArrayStr);
	System.out.println("******this is dividing line******");
	/* Method:the second */
	for (int x = 0; x < arrayStr.size() - 1; ++x) {
		for (int y = x + 1; y < arrayStr.size(); ++y) {
			if (arrayStr.get(x).equals(arrayStr.get(y))) {
				arrayStr.remove(y);
				// 目的是为了解决相同元素，叠上来的时候造成数据漏检。
				y--;
			}
		}
	}
	System.out.println("Result:" + arrayStr);
}
```

- 代码：遍历对象集合，仅保留重复元素的其中一个.

```java
Package TestAa;

importrt java.util.ArrayList;
importrt java.util.Iterator;

s Animal {
private int age;
private String name;

public Animal() {
	super();
	// TODO Auto-generated constructor stub
}

public int getAge() {
	return age;
}

public void setAge(int age) {
	this.age = age;
}

public String getName() {
	return name;
}

public void setName(String name) {
	this.name = name;
}

public Animal(String name, int age) {
	super();
	this.age = age;
	this.name = name;
}

@Override
public String toString() {
	// TODO Auto-generated method stub
	return this.getName() + ":" + this.getAge();
}

// contain使用的是equal方法，所以需要重写equal方法。
@Override
public boolean equals(Object obj) {
	if (this == obj)
		return true;
	if (obj == null)
		return false;
	if (getClass() != obj.getClass())
		return false;
	Animal other = (Animal) obj;
	if (age != other.age)
		return false;
	if (name == null) {
		if (other.name != null)
			return false;
	} else if (!name.equals(other.name))
		return false;
	return true;
}
}

public class CompareAa {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		ArrayList arrayList = new ArrayList();
		Animal animal1 = new Animal("Tom", 3);
		Animal animal2 = new Animal("Jack", 5);
		Animal animal3 = new Animal("Tom", 2);
		Animal animal4 = new Animal("Tom", 3);
		Animal animal5 = new Animal("Jack", 5);
		Animal animal6 = new Animal("Tom", 2);
		Animal animal7 = new Animal("Tom", 2);
		Animal animal8 = new Animal("Jack", 5);
		Animal animal9 = new Animal("Tom", 2);
		arrayList.add(animal1);
		arrayList.add(animal2);
		arrayList.add(animal3);
		arrayList.add(animal4);
		arrayList.add(animal5);
		arrayList.add(animal6);
		arrayList.add(animal7);
		arrayList.add(animal8);
		arrayList.add(animal9);

		/* method:the first... */
		Iterator it = arrayList.iterator();
		ArrayList newArray = new ArrayList();
		while (it.hasNext()) {
			Animal tempCc = (Animal) it.next();
			if (!(newArray.contains(tempCc)))
				newArray.add(tempCc);
		}
		System.out.println(newArray);
		System.out.println("*****this is a dividing line******");
		/* method:the second... */
		for (int x = 0; x < arrayList.size() - 1; ++x) {
			for (int y = x + 1; y < arrayList.size(); ++y) {
				if (arrayList.get(x).equals(arrayList.get(y))) {
					arrayList.remove(y);
					// 目的是为了解决相同元素漏检的问题。
					y--;
				}
			}
		}
		System.out.println(arrayList);

	}
}
```

- 用LinkedList模拟栈结构.

```java
/*for example*/
package TestAa;		
import java.util.LinkedList;	

class StackCc {
	private LinkedList linkCc;

	public StackCc() {
		linkCc = new LinkedList();
		// TODO Auto-generated constructor stub
	}

	public Object popCc() {
		return linkCc.removeFirst();
	}

	public void pushCc(Object obj) {
		linkCc.addFirst(obj);
	}

	@Override
	public String toString() {
		return "StackCc [linkCc=" + linkCc + "]";
	}
}

public class CompareAa {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		StackCc Cc = new StackCc();
		Cc.pushCc("hello");
		Cc.pushCc("world");
		System.out.println(Cc);
		System.out.println("******this is a dividing line******");
		Cc.popCc();
		Cc.popCc();
		System.out.println(Cc);
	}		
}
```

## 泛型
**描述：**是一种把明确类型的工作推迟到创建对象或者调用方法的时候才去明确的特殊的类型，可以理解为一个程序的模版，在需要的时候才将所需要的数据类型填入其中，而在模版需要数据类型的位置需要添加一个占位标记符号<>，<font size = 3, color = 0X6FFF>该数据类型只能是引用类型</font>。
- 出现时间：JDK1.5
- 特点：
	- 把运行时期的问题提前到了编译期间
	- 避免了强制类型转换
	- 优化了程序设计，解决了黄色警告线问题，让程序更安全，上诉的所有代码都有黄色警告，而使用泛型可以解决该问题，比如：已经确定ArrayList数据类型是String，防止list(new Integer(100))，导致下面强转时候报错。
	- 安全性描述：早期的时候，我们使用Object来代表任意的类型，因为其为超类，是类的父类，但是实际使用的时候其实是限制了数据类型了，不能任意的转化，只能转化为本身，这就是存在的安全隐患，也就是警告提示的原因。 也就是说这样的程序其实并不是安全的。所以Java在JDK5后引入了泛型，提高程序的安全性。
	- 代码实例
		- 泛型&String数据类型 
		```java
		/*for example*/
		public static void main(String[] args) {
			//创建对象时候指定添加到数据类型就是String数据类型。		
			ArrayList<String> array = new ArrayList<String>();
				
			array.add("hello");
			array.add("world");	
				
			Iterator<String> it = array.iterator();
			while (it.hasNext()) {			
				String s = it.next();
				System.out.println(s);
			System.out.println("--------this is a dividing ling--------");
	
			for (String Cc : array) {
				System.out.println(Cc);
			}
			System.out.println("-------this is a dividing ling-------");
			for (int x = 0; x < array.size(); ++x) {
				System.out.println(array.get(x));
			}
		}
		/*  泛型就是事先确定数据类型，不能具体确定的数据类型就使用占位符-通配符描述。*/
		```

		- 泛型&对象数据类型
		```
		/*数据对象类型为对象类的遍历实现*/
		package TestAa;
		
		import java.util.ArrayList;
		import java.util.Iterator;
		
		class Animal {
			private String name;
			private int age;
		
			public Animal(String name, int age) {
				super();
				this.name = name;
				this.age = age;
			}
		
			public Animal() {
				super();
				// TODO Auto-generated constructor stub
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
		}
		
		public class CompareAa {		
			/**
			 * @param args
			 */		
			public static void main(String[] args) {
				// JDK7的新特性：泛型推断。
				// ArrayList<Student> array = new ArrayList<>();不会报错也不会警告，但是不建议这样使用。
				ArrayList<Animal> array = new ArrayList<>();
		
				Animal s1 = new Animal("Tom", 4);
				Animal s2 = new Animal("Jack", 2);
				Animal s3 = new Animal("Coffe", 5);
		
				array.add(s1);
				array.add(s2);
				array.add(s3);
		
				Iterator<Animal> it = array.iterator();
				while (it.hasNext()) {
					Animal s = it.next();
					System.out.println(s.getName() + "---" + s.getAge());
				}
				System.out.println("------this is a dividing line------");
		
				for (int x = 0; x < array.size(); x++) {
					Animal s = array.get(x);
					System.out.println(s.getName() + "---" + s.getAge());
				}
				System.out.println("------this is a dividing line------");
				for (Animal s : array) {
					System.out.println(s.getName() + "---" + s.getAge());
				}
			}
		}
		
		```


- 类别
	- 泛型类
		```java
		/* for example*/
		/*该段在编译时候不会报错也不会警告，但是运行错误，原因是类型转化问题*/
		public class ObjectTool {
			private Object obj;
		
			public Object getObj() {
				return obj;
			}
		
			public void setObj(Object obj) { 
				// Object obj = new Integer(30);
				this.obj = obj;
			}
		}
		public class ObjectToolDemo {
			public static void main(String[] args) {
				ObjectTool ot = new ObjectTool();
		
				// 运行正常。
				ot.setObj(new Integer(2));
				Integer i = (Integer) ot.getObj();
				System.out.println("年龄是：" + i);
		
				ot.setObj(new String("Tom"));
				String s = (String) ot.getObj();
				System.out.println("姓名是：" + s);
		
				System.out.println("---------");
				ot.setObj(new Integer(2));
				// 报错，类型转换错误，ClassCastException
				String ss = (String) ot.getObj();
				System.out.println("姓名是：" + ss);
			}
		}
		System.out.println("-------this is a dividing line---------");
		/*泛型解决方案*/
		public class ObjectTool<T> {
			private T obj;
		
			public T getObj() {
				return obj;
			}
		
			public void setObj(T obj) {
				this.obj = obj;
			}
		}
		public class ObjectToolDemo {
			public static void main(String[] args) {
		
				ObjectTool<String> ot = new ObjectTool<String>();		
				ot.setObj(new String("Tom"));
				String s = ot.getObj();
				System.out.println("姓名是：" + s);
		
				ObjectTool<Integer> ot2 = new ObjectTool<Integer>();
				// ot2.setObj(new String("Jack"));//这个时候编译期间就过不去
				ot2.setObj(new Integer(27));
				Integer i = ot2.getObj();
				System.out.println("年龄是：" + i);
			}
		}
		```

	- 泛型方法
		```java
		public class ObjectTool {
			public <T> void show(T t) {
				System.out.println(t);
			}
		}
		public class ObjectToolDemo {
			public static void main(String[] args) {
				// 定义到泛型方法后，可以接受任意数据类型。
				ObjectTool ot = new ObjectTool();
				ot.show("hello");
				ot.show(100);
				ot.show(true);
			}
		}
		```
	- 泛型接口
		```java
		public interface Inter<T> {
			public abstract void show(T t);
		}
		/*第一种：实现类在实现接口的时候已经知道数据类型*/
		
		public class InterImpl implements Inter<String> {	
			@Override
			public void show(String t) {
				System.out.println(t);
			}
		 }
		
		//第二种情况：还不知道数据类型.
		public class InterImpl<T> implements Inter<T> {
		
			@Override
			public void show(T t) {
				System.out.println(t);
			}
		}
		
			public static void main(String[] args) {
				/*第一种*/
				 Inter<String> i = new InterImpl();
				i.show("hello");
		
				/*第二种情况的测试*/
				Inter<String> i = new InterImpl<String>();
				i.show("hello");
		
				Inter<Integer> ii = new InterImpl<Integer>();
				ii.show(100);
			}
		
		```

- 泛型高级通配符
	- ?
	- ? extends E
	- ? super E
	- 代码实例
		```java
		/*for example*/
		public class GenericDemo {
			public static void main(String[] args) {
				// 泛型如果明确的写的时候，前后必须一致
				Collection<Object> c1 = new ArrayList<Object>();
				// Collection<Object> c2 = new ArrayList<Animal>();	报错	
		
				// ?表示任意的类型都是可以的
				Collection<?> c5 = new ArrayList<Object>();		
				Collection<?> c6 = new ArrayList<Animal>();			
		
				// ? extends E:向下限定，E及其子类
				// Collection<? extends Animal> c9 = new ArrayList<Object>();
				Collection<? extends Animal> c10 = new ArrayList<Animal>();
				Collection<? extends Animal> c11 = new ArrayList<Dog>();		
		
				// ? super E:向上限定，E极其父类
				Collection<? super Animal> c13 = new ArrayList<Object>();
				Collection<? super Animal> c14 = new ArrayList<Animal>();
				// Collection<? super Animal> c15 = new ArrayList<Dog>();
				
			}
		}
		
		class Animal {
		}
		class extends Dog{
		}
		```
	
### 增强for循环
**描述：**一种简化的for循环结构。
- 语法格式
```
for(DataType Variable : Collection/Object){
	Variable in Array/Collection.
}
```
### 可变参数
Arrays工具下一个函数：
static <T> List<T> asList(T... a) 返回由指定数组支持的固定大小的列表。
- 代码实例
```
List<String> list = Arrays.asList("hello", "world");
	// UnsupportedOperationException
	// list.add("NewDay");
	// UnsupportedOperationException
	// list.remove(NewLife);
	list.set(1, "Test");

	for (String s : list) {
		System.out.println(s);
	}
}
```
###  集合嵌套遍历
- 实例一
	```
	/*for example,二层嵌套*/
	public class CollectionCc {
		public static void main(String[] args) {
			// 最外层集合
	
			ArrayList<ArrayList<Animal>> bigArrayList = new ArrayList<ArrayList<Animal>>();
			//创建对应的所有对象，分别添加进组，再小组进大组。
			ArrayList<Animal> firstArrayList = new ArrayList<Student>();	
			bigArrayList.add(firstArrayList);
		
			ArrayList<Animal> secondArrayList = new ArrayList<Student>();
			bigArrayList.add(secondArrayList);
		
			ArrayList<Animal> thirdArrayList = new ArrayList<Student>();
			bigArrayList.add(thirdArrayList);
	
			// 遍历集合
			for (ArrayList<Animal> array : bigArrayList) {
				for (Student s : array) {
					System.out.println(s.getName() + "---" + s.getAge());
				}
			}
		}
	}
	```

- 实例二
	```
	/*获取1-100随机数*/
	public static void main(String[] args) {		
			Random r = new Random();
			
			//泛型集合
			ArrayList<Integer> arrayCc = new ArrayList<Integer>();		
			int count = 0;
			
			while (count < 10) {			
				int number = r.nextInt(20) + 1;			
				
				if(!arrayCc.contains(number)){
					//如果不存在:就添加，统计变量++。
					arrayCc.add(number);
					count++;
				}
			}		
			//增强for遍历.
			for(Integer i : array){
				System.out.println(i);
			}
			System.out.println(array);
		}
	```

- 实例三
	```
	/*for example*/
	/*获取键盘输入的不确定个数的最大值.*/
	public static void main(String[] args) {		
		Scanner sc = new Scanner(System.in);
	
		// 数据不确定的情况下。只能是用集合存储，翻新保证程序的健壮性.
		ArrayList<Integer> array = new ArrayList<Integer>();
	
		// 设置结束符号，此处为0.
		while (true) {
			System.out.println("请输入数据：");
			int number = sc.nextInt();
			if (number != 0) {
				array.add(number);
			} else {
				break;
			}
		}
	
		// 把集合转成数组
		// public <T> T[] toArray(T[] a)
		Integer[] i = new Integer[array.size()];
		// Integer[] ii = array.toArray(i);
		array.toArray(i);
	
		// 对数组排序,public static void sort(Object[] a).
		Arrays.sort(i);
	
		// 获取该数组中的最大索引的值
		System.out.println("数组是：" + arrayToString(i) + ",最大值是:"
				+ i[i.length - 1]);
		}
	
		public static String arrayToString(Integer[] i) {
			StringBuilder sb = new StringBuilder();
	
			sb.append("[");
			for (int x = 0; x < i.length; x++) {
				if (x == i.length - 1) {
					sb.append(i[x]);
				} else {
					sb.append(i[x]).append(", ");
				}
			}
			sb.append("]");
	
			return sb.toString();
		}	
	```