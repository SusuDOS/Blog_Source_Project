---
title: HTML前端(一)----DIV布局
date: 2019-01-18 22:41:34
categories: HTML
tags: [HTML的DIV布局, 非CSS实现]
---




### HTML的基本结构

- 使用sublime的快捷键生成HTML的模版文件格式.
- 生成HTMl5.0的文件格式 `! + tab`

```html
<!-- 这个是注释符号测试. -->
<!-- 在sublime下输入！再按一下tab键即可快速生成. -->
<!DOCTYPE html>
<html lang="en">
    <head>            
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>          
    </body>
</html>

```
- 生成HTML1.0的文件格式 `html:xt +tab`

```html
<!-- html:xt，再按一下tab键即可快速实现文件的格式生成. -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
<head>
    <meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
    <title> xhtml 1.0  </title>
</head>
<body>
</body>
</html>
```

- 说明
	- HTML一般是使用成对的 `<*></*>` 来描述的，但是有极少数的例外是不需要的。

```html
<!DOCTYPE html>
<html lang="en">
    <head>            
        <meta charset="UTF-8">
        <title>这个是网页标题</title>
    </head>
    <body>
		这个是网页的主体部分，用来描述网页的内容的。          
    </body>

<!-- 这个是网页的结束，成对的出现，与开始部分是想对应的. -->
</html>
```

<font color=red>

### 若需要使用快捷键生成，则需要安装对应的插件.

推荐安装的插件如下：

- Emmet
目前，Emmet 已经可以通过 Package Control 安装了。如果你的 Sublime Text 3 还没有安装 Package Control，请参照以下方法安装。

</font>

- 为 `Sublime Text 3` 安装 `Package Control` 组件：

按 `Ctrl + `` 打开控制台，粘贴以下代码到底部命令行并回车如下：

```html
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp =   sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())

```

之后在左下角看到提示，也就是没有 `=` 来回小幅度滑动，此时退出sublime，注意退出方式是file->exit ,在 Perferences->Package Settings 中看到 `Package Control`，则表示安装成功.

在 Perferences->Package Settings 中看到 Package Control，则表示安装成功.

- 使用 Package Control 安装 Emmet 插件：

在 Package Control 中选择 Install package 或者按 Ctrl+Shift+P，打开命令板输入 pci 然后选择 Install Package,也就是单击即可安装，(若是没有找到说明已经完成安装)，同时左小角有一个小的 `=` 小幅度滑动.

- 安装emmet的插件

	- 搜索输入 emmet 找到 Emmet Css Snippets，点击就可以自动完成安装。

<font color=blue>插件安装部分完成，若是不成功，理由只有一个就是你的数据无法找到，可以通过vpn实现自己的安装，没有vpn使用代理连接墙外服务器的几乎失败。</font>

### 购买外国的VPS安装VPN代理

- 推荐服务器提供商-搬瓦工，使用优惠劵后，年费约198人民币。单核-500MB内存-1Mb/s-10GB SSD硬盘+500GB/mon。比较便宜，应该是最便宜的了，但是完全可以实现一个人的正常使用，看看720p视频之类的还是完全无压力的。
	
- 方法自己找，但是可以借鉴此人的：[https://juejin.im/post/5b14c5115188257d37761a5a](https://juejin.im/post/5b14c5115188257d37761a5a "https://juejin.im/post/5b14c5115188257d37761a5a")


### HTML标签

- 段落标签

```html
<p>&nbsp;&nbsp;&nbsp;（空格，三个空格略等于一个中文汉字占据宽度，此法的空格是不精确的，用css样式可以精确定义.）<br />


<!-- 这个是换行符号. -->
<br />

这个是段落标签，带样式，带样式的意思是不仅仅是显示文档信息还有段落的格式问题，如段落缩进，行间距等。</p>

```
- 块标签

	- `<div>` 标签 块元素，表示一块内容，没有具体的语义-无样式。
	- `<span>` 标签 行内元素，表示一行中的一小段内容，没有具体的语义-无样式。

- 插入-图片链接

```html
<img src="images/logo.png" alt="百度logo">
```

- 插入-网址超链接

```html
<a href="这是一个新的网址.html">点我去新网址！</a>
```

- 点图片超链接到网页地址

```html
<a href="http://www.itcast.cn" title="去到百度的网站" target="_blank"><img src="images/logo.png" alt="百度logo">
```

- 列表标签

```html

<!-- 快速生成有序列表，会标有1,2,3...如下： -->
	<!-- ol>li*3 + tab -->

	<ol>
		<li>html</li>
		<li>css</li>
		<li>javascript</li>
	</ol>


<!-- 快速生成无序列表，没有标号，只有小黑点，如下： -->
	<!-- ul>(li>a{标题})*3  -->
	<ul>
		<li><a href="#">标题</a></li>
		<li><a href="#">标题</a></li>
		<li><a href="#">标题</a></li>
	</ul>
```

### HTML-表格

1、`<table>`标签：声明一个表格，它的常用属性如下：

- border属性 定义表格的边框，设置值是数值.
- cellpadding属性 定义单元格内容与边框的距离，设置值是数值.
- cellspacing属性 定义单元格与单元格之间的距离，设置值是数值.
- align属性 设置整体表格相对于浏览器窗口的水平对齐方式,设置值有：`left` | `center` | `right`。

2、`<tr>`标签：定义表格中的一行.

3、`<td>`和`<th>`标签：定义一行中的一个单元格，td代表普通单元格，th表示表头单元格，它们的常用属性如下：

- align 设置单元格中内容的水平对齐方式,设置值有：`left` | `center` | `right`.
- valign 设置单元格中内容的垂直对齐方式 `top` | `middle` | `bottom`.
- colspan 设置单元格水平合并，设置值是合并的列数值.
- rowspan 设置单元格垂直合并，设置值是合并的行数值.

<details>

<font color=blue><summary>点击:代码展开</summary></font>

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>表格</title>
</head>
<body>
	<h1>表格</h1>

	<!-- cellspacing设置为0，则表格线合并. -->
	<table  width="700" height="300" align="center" border="1" cellpadding="20" cellspacing="0">
		<tr>
			<td>1</td>
			<td>2</td>
			<td>3</td>
			<td>4</td>
		</tr>
		<tr>

			<!-- 此单元格居中对齐 -->
			<td align="center">1</td>
			<td>2</td>
			<td>3</td>
			<td>4</td>
		</tr>
		<tr>
			<td>1</td>
			<td>2</td>
			<td>3</td>
			<td>4</td>
		</tr>
		<tr>
			<td>1</td>
			<td>2</td>
			<td>3</td>
			<td>4</td>
		</tr>
	</table>

	<!-- 这是为二者之间添加空行. -->
	<br />

	<!-- table>(tr>td*5)*6 -->
	<table border="1" width="700" height="300" align="center">
		<tr>
			<td colspan="5">基本情况</td>			
		</tr>
		<tr>
			<td width="15%"></td>
			<td width="25%"></td>
			<td width="15%"></td>
			<td width="25%"></td>
			<td rowspan="5"><img src="images/person.png" alt="人物图片"></td>
		</tr>
		<tr>
			<td></td>
			<td></td>
			<td></td>
			<td></td>			
		</tr>
		<tr>
			<td></td>
			<td></td>
			<td></td>
			<td></td>
			
		</tr>
		<tr>
			<td></td>
			<td></td>
			<td></td>
			<td></td>
			
		</tr>
		<tr>
			<td></td>
			<td></td>
			<td></td>
			<td></td>
			
		</tr>
	</table>

</body>
</html>
```
</details>

- 结合使用总体效果

![](https://i.imgur.com/32WKpjX.png)


**<font color=red>综合使用 表格实现各项简历</font>**

<details>

<font color=blue><summary>点击:代码展开</summary></font>
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>个人简历</title>
</head>
<body topmargin="0">

	<table width="800" height="800" border="0" cellpadding="0" cellspacing="0" align="center">
		<tr>
			<td width="260" valign="top" bgcolor="#f2f2f2">

				
				<table width="200" border="0" cellpadding="0" cellspacing="0" align="center">
					<tr>
						<td height="100"></td>
					</tr>
					<tr>
						<td align="right"><img src="images/person.png"></td>
					</tr>
					<tr>
						<td align="right">测试者</td>
					</tr>
					<tr>
						<td align="right">12300456789</td>
					</tr>
					<tr>
						<td align="right">123456789@126.com</td>
					</tr>
				</table>


			</td>
			<td width="30"></td>
			<td width="480" valign="top">
				
				<table width="480" border="0" cellpadding="0" cellspacing="0">
					<tr><td height="80"></td></tr>
					<tr>
						<td align="right"><img src="images/resume.png"></td>
					</tr>
				</table>
				<br>
				<hr />
				<br>

				<table width="480" height="200" border="0" cellpadding="0" cellspacing="0">
					<tr>
						<td colspan="2"><b>个人基本情况</b></td>					
					</tr>
					<tr>
						<td><b>姓 名：</b>测试者	</td>
						<td><b>籍 贯：</b>测试</td>
					</tr>
					<tr>
						<td><b>性 别：</b>男</td>
						<td><b>身 高：</b>174cm</td>
					</tr>
					<tr>
						<td><b>民 族：</b>汉</td>
						<td><b>体 重：</b>71kg</td>
					</tr>
					<tr>
						<td><b>出生日期：</b>2018.03.28	</td>
						<td><b>电 话：</b>18210898888</td>
					</tr>
					<tr>
						<td><b>现居住地：</b>测试</td>
						<td><b>专 业：</b>测试组</td>
					</tr>
				</table>
				<br>
				<br>

				<table width="480" height="80" border="0" cellpadding="0" cellspacing="0">
					<tr>
						<td><b>教育背景及工作经历</b></td>
					</tr>
					<tr>
						<td><b>2008.09-2011.06</b> ****大学 工业设计专业</td>
					</tr>
					<tr>
						<td><b>2011.06-2012.09</b>**** html</td>
					</tr>


				</table>

					<br>
				<br>

				<table width="480" height="80" border="0" cellpadding="0" cellspacing="0">
					<tr>
						<td><b>所获证书</b></td>
					</tr>
					<tr>
						<td><b>2009年：</b> 荣获“高级美术设计师”证书</td>
					</tr>
					<tr>
						<td><b>2009年：</b>荣获“优秀班干部”证书</td>
					</tr>


				</table>

			</td>
			<td width="30"></td>
		</tr>
	</table>


</body>
</html>
```
</details>

- 以上代码效果一览

![](https://i.imgur.com/jEBRpE7.png)

![](https://i.imgur.com/LxRaABL.png)



### HTML-表单


表单用于搜集不同类型的用户输入，表单由不同类型的标签组成，相关标签及属性用法如下：

1、`<form>`标签 定义整体的表单区域

- action属性 定义表单数据提交地址.
- method属性 定义表单提交的方式，一般有“get”方式和“post”方式.

2、`<label>`标签 为表单元素定义文字标注.

3、`<input>`标签 定义通用的表单元素.

有如下属性：

- type="text" 定义单行文本输入框.
- type="password" 定义密码输入框.
- type="radio" 定义单选框.
- type="checkbox" 定义复选框.
- type="file" 定义上传文件.
- type="submit" 定义提交按钮.
- type="reset" 定义重置按钮.
- type="button" 定义一个普通按钮.
- type="image" 定义图片作为提交按钮，用src属性定义图片地址.
- type="hidden" 定义一个隐藏的表单域，用来存储值.
- value属性 定义表单元素的值.
- name属性 定义表单元素的名称，此名称是提交数据时的键名.
4、`<textarea>`标签 定义多行文本输入框.

5、`<select>`标签 定义下拉表单元素.

6、`<option>`标签 与`<select>`标签配合，定义下拉表单元素中的选项.

#### 表单实例

<details>

<font color=blue><summary>点击:代码展开</summary></font>
```html
<!DOCTYPE html>
	<html lang="en">
	<head>
		<meta charset="UTF-8">
		<title>表单</title>
	</head>
	<body>
		<h1>注册表单</h1>
	
		<!-- 表单的数据类型众多. -->
	
		<!-- 提交当前的内容，方式为post. -->
		<form action="" method="post">		
			<div>
	
				<!-- for的功能是：实现点击用户名这几个字，鼠标开始定位到输入框. -->
	
				<!-- label为普通的标签. -->
				<label for="username">用户名：</label>
				<input type="text" name="username" id="username" />
			</div>		
			<br>
			<div>
				<label for="password">密&nbsp;&nbsp;&nbsp;码：</label>
	
				<!-- 类型为password，则输入后，将值显示为小黑点. -->
				<input type="password" name="password" id="password">
			</div>
			<br>
			<div>
				<label>性&nbsp;&nbsp;&nbsp;别：</label>
				<input type="radio" name="gender" value="0" id="male"> <label for="male">男</label>
				<input type="radio" name="gender" value="1" id="female"> <label for="female">女</label>
			</div>
			<br>
			<div>
				<label>爱&nbsp;&nbsp;&nbsp;好：</label>
	
				<!-- 设置value值，目的是为了提交上去。 -->
				<input type="checkbox" name="like" value="eat"> 吃饭
				<input type="checkbox" name="like" value="sleep"> 睡觉
				<input type="checkbox" name="like" value="play"> 打游戏
				<input type="checkbox" name="like" value="movie"> 看电影
	
			</div>
	
			<br>
	
			<div>
				<label>照&nbsp;&nbsp;&nbsp;骗：</label>
				<input type="file" name="">
			</div>
			<br>
	
			<div>
				<label>个人介绍：</label>
				<textarea name="introduce"></textarea>
			</div>
			
			<br>
	
			<div>
				<label>籍&nbsp;&nbsp;&nbsp;贯：</label>
				<select name="site">
					<option value="0">北京</option>
					<option value="1">上海</option>					
					<option value="2">河南</option>	
					<option value="3">河北</option>	
					<option value="4">广州</option>	
				</select>
	
			</div>
			<br>
	
			<input type="hidden" name="hid01" value="12">
	
			<div>
	
				<input type="submit" name="" value="提交">
	
				<!-- 以下的方式不推荐使用. -->
	
				<!-- <input type="image" src="images/goods.jpg" name=""> -->
	
	
				<input type="reset" name="" value="重置">
			</div>
		</form>
	</body>
</html>
```  
</details>

- 效果一览

![](https://i.imgur.com/37Xhmt5.png)