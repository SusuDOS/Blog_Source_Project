---
title: Collection&Ma&Poker
date: 2018-11-2 21:45:49
categories: Java基础学习
tags: [Collection Ma Poker]
---
## Map
**描述：**
>键映射到值的对象, 地图不能包含重复的键; 每个键可以映射到最多一个值。 
这个接口取代了Dictionary类，它是一个**完全抽象的类**而不是接口。
>
- Map&Collection区别
	- Map 存储的是键值对形式的元素，键唯一，值可以重复。
	- Collection 存储的是单独出现的元素，子接口Set元素唯一，子接口List元素可重复。
- Map接口功能
	- 添加功能
		```java
		V put(K key,V value):添加元素。
			若存在则返回更新之前的value值，不存在返回NULL，且属于可选择返回：可以返回，也可以不返回。
		
		/*for example*/
		Map<String, String> map = new HashMap<String, String>();
		System.out.println(map.put("Tom", "2"));	//NULL
		System.out.println(map.put("Tom", "3"));	//2
		```
	- 删除功能
		```java
		void clear():移除所有的键值对元素.
		V remove(Object key)：根据键删除键值对元素,并把值返回.
		System.out.println("remove:" + map.remove("Tom));	//3
		```
	- 判断功能
		```java
		boolean containsKey(Object key)：判断集合是否包含指定的键.
		boolean containsValue(Object value):判断集合是否包含指定的值.
		boolean isEmpty()：判断集合是否为空.
		```
	- 获取功能
		```java
		Set<Map.Entry<K,V>> entrySet():返回键值对.
			/*for example*/
			Set<Map.Entry<String, String>> set = map.entrySet();

		V get(Object key):根据键获取值.
			/*for example*/
			System.out.println("get:" + map.get("Tom"));	//显示3.

		Set<K> keySet():获取集合中所有键的集合.
			/*for example*/
			Set<String> set = map.keySet();

		Collection<V> values():获取集合中所有值的集合.
			/*for example*/
			Collection<String> con = map.values();

		/*conclusion*/
		Map<String, String> map = new HashMap<String, String>();
		Set<Map.Entry<String, String>> set = map.entrySet();		
				for (Map.Entry<String, String> Aa : set) {			
					String key = Aa.getKey();
					String value = Aa.getValue();
					System.out.println(key + "---" + value);
				}
				
		```

	- 长度功能
		```java
		int size()：返回集合中的键值对的对数.
				/*for example*/
				System.out.println("size:"+map.size());
		```


- Map集合的遍历
	- 键->值.

		```java
		/*for example*/
		Map<String, String> map = new HashMap<String, String>();
			Set<String> set = map.keySet();
				
			for (String key : set) {			
				String value = map.get(key);
				System.out.println(key + "---" + value);
		}
		```
	- 键值对->找键和值
		```java
		Map<String, String> map = new HashMap<String, String>();		
		Set<Map.Entry<String, String>> set = map.entrySet();
			/*遍历键值对*/
			for (Map.Entry<String, String> me : set) {			
				String key = me.getKey();
				String value = me.getValue();
				System.out.println(key + "---" + value);
		}
		```
			
- HashMap集合
	- HashMap<String,String>
		```java
			HashMap<String, String> hm = new HashMap<String, String>();
			
			键值对添加区域-	//hm.put("key","value")
		
			/*遍历*/
			Set<String> set = hm.keySet();
			for (String key : set) {
				String value = hm.get(key);
				System.out.println(key + "---" + value);
			}
		```
	- HashMap<Integer,String>

		```java
		HashMap<Integer, String> hm = new HashMap<Integer, String>();
		
			-键值对添加区域-	//hm.put(key,"value")
		
			// 遍历
			Set<Integer> set = hm.keySet();
			for (Integer key : set) {
				String value = hm.get(key);
				System.out.println(key + "---" + value);
			}
		```
	- HashMap<String,Student>
	
		```java
		HashMap<String, Student> hm = new HashMap<String, Student>();
		
			-键值对添加区域-	
				//hm.put("key",value) value=objecet对象，如student.
		
			// 遍历
			Set<String> set = hm.keySet();
			for (String key : set) {			
				Student value = hm.get(key);
				System.out.println(key + "---" + value.getName() + "---"
						+ value.getAge());
		}
		```

	- HashMap<Student,String>
		```java
		HashMap<Student, String> hm = new HashMap<Student, String>();
		
				-键值对添加区域-	
						//hm.put(value,"key") value=objecet对象，如student.	
		
				// 遍历
				Set<Student> set = hm.keySet();
				for (Student key : set) {
					String value = hm.get(key);
					System.out.println(key.getName() + "---" + key.getAge() + "---"
							+ value);
				}
		```


 - TreeMap集合
	- TreeMap<String,String>
		```java
		TreeMap<String, String> tm = new TreeMap<String, String>();
			
			-键值对添加区域-	
							//hm.put("key","value") 
		
			// 遍历集合
			Set<String> set = tm.keySet();
			for (String key : set) {
				String value = tm.get(key);
				System.out.println(key + "---" + value);
			}
		```
	- TreeMap<Student,String>
		
		```java
		/*for example*/
		
		/*自动生成学生类*/
		public class Student {
			private String name;
			private int age;
		
			public Student() {
				super();
			}
		
			public Student(String name, int age) {
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
		}
		
		/*测试主类*/
		import java.util.Comparator;
		import java.util.Set;
		import java.util.TreeMap;
		
		/*TreeMap<Student,String>*/
		
		public class TreeMapMain {
			public static void main(String[] args) {
		
			//匿名抽象借口重写
			TreeMap<Student, String> tm = new TreeMap<Student, String>(
					new Comparator<Student>() {					
						@Override
						public int compare(Student s1, Student s2) {
							//第一个变量-第二个=升序排列.
							// 优先级年龄
							int num = s1.getAge() - s2.getAge();
							// 优先级名字
							int num2 = num == 0 ? s1.getName().compareTo(
									s2.getName()) : num;
							return num2;
						}
					});
			
			Student s1 = new Student("Aa", 30);
			Student s2 = new Student("Bb", 35);
			Student s3 = new Student("Cc", 33);
			Student s4 = new Student("Dd", 32);
			Student s5 = new Student("Ee", 33);
			
			tm.put(s1, "01");
			tm.put(s2, "02");
			tm.put(s3, "03");
			tm.put(s4, "04");
			tm.put(s5, "05");
		
			// 遍历
			Set<Student> set = tm.keySet();
			for (Student key : set) {
				String value = tm.get(key);
				System.out.println(key.getName() + "---" + key.getAge() + "---"
						+ value);
			}
			}
		}		
		```
- 多层嵌套
	- hashMap&hashMap
		```java
		/*for example*/
		//注意导包:Ctrl+Shift+O
		
		package TestAa;
		
		import java.util.HashMap;
		import java.util.Set;
		
		public class HashMapHashMap {
			public static void main(String[] args) {
		
				HashMap<String, HashMap<String, Integer>> bigMap = new HashMap<String, HashMap<String, Integer>>();
		
				// 子图一
				HashMap<String, Integer> oneMap = new HashMap<String, Integer>();
				oneMap.put("Tom", 5);
				oneMap.put("Jack", 2);
				bigMap.put("one", oneMap);
		
				// 子图二
				HashMap<String, Integer> twoMap = new HashMap<String, Integer>();
				twoMap.put("John", 3);
				twoMap.put("Suqi", 4);
				bigMap.put("two", twoMap);
		
				// 遍历集合
				Set<String> bigMapSet = bigMap.keySet();
				for (String bigMapKey : bigMapSet) {
					System.out.println(bigMapKey);
		
					HashMap<String, Integer> bigMapValue = bigMap.get(bigMapKey);
					Set<String> sonMapSet = bigMapValue.keySet();
					for (String sonValue : sonMapSet) {
						Integer value = bigMapValue.get(sonValue);
						System.out.println("\t" + sonValue + "---" + value);
					}
				}
			}
		}
		
		```

	- 统计字符串字母频数

		```java
		public class TreeMap{
			public static void main(String[] args) {
				
				Scanner sc = new Scanner(System.in);
				System.out.println("请输入一个字符串：");
				String line = sc.nextLine();
				
				TreeMap<Character, Integer> tm = new TreeMap<Character, Integer>();		
				char[] chs = line.toCharArray();
				
				for(char ch : chs){			
					Integer i =  tm.get(ch);			
					if(i == null){
						tm.put(ch, 1);
					}else {				
						i++;
						tm.put(ch,i);
					}
				}
						
				StringBuilder sb=  new StringBuilder();	
				
				Set<Character> set = tm.keySet();
				for(Character key : set){
					Integer value = tm.get(key);
					sb.append(key).append("(").append(value).append(")");
				}		
				String result = sb.toString();
				System.out.println("result:"+result);
			}
		}
		```
- 忽略以下.
	- HashMap嵌套ArrayList
	- ArrayList嵌套HashMap
	- 多层嵌套
			
## Collections
描述：注意与collection的区别,是针对集合进行操作的工具类.
- Collection&Collections差异
	- Collection 是单列集合的顶层接口，有两个子接口List和Set.
	- Collections 是针对集合进行操作的工具类，可以对集合进行排序和查找等.
- 常用函数
	- public static <T> void sort(List<T> list)

		```
		List<Integer> list = new ArrayList<>();
		list.add(value);			//value为Integer数据类型.
		Collections.sort(list);
		```
	- public static <T> int binarySearch(List<?> list,T key)

		```
		/*二分查找法，返回Index，未找到为-[(List.size)+1]*/
		System.out.println("binarySearch:" + Collections.binarySearch(list, 60));
		```
	- public static <T> T max(Collection<?> coll)

		```
		/*for example，获取最大值.*/
		System.out.println("max:" + Collections.max(list));
		```
	- public static void reverse(List<?> list)

		```
		Collections.reverse(list);	//List反转
		System.out.println("list:" + list);
		```
	- public static void shuffle(List<?> list)
		```
		Collections.shuffle(list);	//List随机打乱.
		System.out.println("list:" + list);
		```
- 代码实例：
	- ArrayList对象排序
		```
		public class CollectionsDemo {
			public static void main(String[] args) {
				
				List<Student> list = new ArrayList<Student>();
				- 添加对象数据区域-
				- 		
				// Collections.sort(list);
				// 比较器排序，如果同时有自然排序和比较器排序，以比较器排序为主
				Collections.sort(list, new Comparator<Student>() {
					@Override
					public int compare(Student s1, Student s2) {
						int num = s2.getAge() - s1.getAge();
						int num2 = num == 0 ? s1.getName().compareTo(s2.getName())
								: num;
						return num2;
					}
				});
				
				for (Student s : list) {
					System.out.println(s.getName() + "---" + s.getAge());
				}
			}
		}
		```

	- 斗地主洗牌&发牌&排序-(来源于网上.)

		```
		package cn.itcast_04;
		
		import java.util.ArrayList;
		import java.util.Collections;
		import java.util.HashMap;
		import java.util.TreeSet;
		
		/*
		 * 思路：
		 * 		A:创建一个HashMap集合
		 * 		B:创建一个ArrayList集合
		 * 		C:创建花色数组和点数数组
		 * 		D:从0开始往HashMap里面存储编号，并存储对应的牌
		 *        同时往ArrayList里面存储编号即可。
		 *      E:洗牌(洗的是编号)
		 *      F:发牌(发的也是编号，为了保证编号是排序的，就创建TreeSet集合接收)
		 *      G:看牌(遍历TreeSet集合，获取编号，到HashMap集合找对应的牌)
		 */
		public class PokerDemo {
			public static void main(String[] args) {
				// 创建一个HashMap集合
				HashMap<Integer, String> hm = new HashMap<Integer, String>();
		
				// 创建一个ArrayList集合
				ArrayList<Integer> array = new ArrayList<Integer>();
		
				// 创建花色数组和点数数组
				// 定义一个花色数组
				String[] colors = { "♠", "♥", "♣", "♦" };
				// 定义一个点数数组
				String[] numbers = { "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q",
						"K", "A", "2", };
		
				// 从0开始往HashMap里面存储编号，并存储对应的牌,同时往ArrayList里面存储编号即可。
				int index = 0;
		
				for (String number : numbers) {
					for (String color : colors) {
						String poker = color.concat(number);
						hm.put(index, poker);
						array.add(index);
						index++;
					}
				}
				hm.put(index, "小王");
				array.add(index);
				index++;
				hm.put(index, "大王");
				array.add(index);
		
				// 洗牌(洗的是编号)
				Collections.shuffle(array);
		
				// 发牌(发的也是编号，为了保证编号是排序的，就创建TreeSet集合接收)
				TreeSet<Integer> fengQingYang = new TreeSet<Integer>();
				TreeSet<Integer> linQingXia = new TreeSet<Integer>();
				TreeSet<Integer> liuYi = new TreeSet<Integer>();
				TreeSet<Integer> diPai = new TreeSet<Integer>();
		
				for (int x = 0; x < array.size(); x++) {
					if (x >= array.size() - 3) {
						diPai.add(array.get(x));
					} else if (x % 3 == 0) {
						fengQingYang.add(array.get(x));
					} else if (x % 3 == 1) {
						linQingXia.add(array.get(x));
					} else if (x % 3 == 2) {
						liuYi.add(array.get(x));
					}
				}
		
				// 看牌(遍历TreeSet集合，获取编号，到HashMap集合找对应的牌)
				lookPoker("风清扬", fengQingYang, hm);
				lookPoker("林青霞", linQingXia, hm);
				lookPoker("刘意", liuYi, hm);
				lookPoker("底牌", diPai, hm);
			}
		
			// 写看牌的功能
			public static void lookPoker(String name, TreeSet<Integer> ts,
					HashMap<Integer, String> hm) {
				System.out.print(name + "的牌是：");
				for (Integer key : ts) {
					String value = hm.get(key);
					System.out.print(value + " ");
				}
				System.out.println();
			}
		}	
		```