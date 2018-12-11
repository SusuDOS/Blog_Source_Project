---
title: npm&github
date: 2018-11-24 23:45:49
categories: GitHub&走过的坑
tags: [npm, npm镜像]
---

npm全称Node Package Manager，是node.js的模块依赖管理工具，主要应用与web前端。由于npm的源在国外，速度慢or无法安装，国内优秀的npm镜像资源可以选择使用。

## 国内优秀npm镜像
### 淘宝npm镜像
- 搜索地址：http://npm.taobao.org/
- registry地址：http://registry.npm.taobao.org/

### cnpmjs镜像
- 搜索地址：http://cnpmjs.org/
- registry地址：http://r.cnpmjs.org/

### 如何使用
有很多方法来配置npm的registry地址，下面根据不同情境列出几种比较常用的方法。以淘宝npm镜像举例：

- 临时使用
 npm --registry https://registry.npm.taobao.org install express
- 持久使用-修改地址.

**Git输入**：npm config set registry https://registry.npm.taobao.org

**文本直接编辑**：registry=https://registry.npm.taobao.org

![](https://i.imgur.com/kLLhBUq.png)

### 偏好使用
- 
	```
	npm i(nstall) (sourcepackage eg:) hexo-generator-archive --save-dev --registry=https://registry.npm.taobao.org
	```
- 配置后验证是配置否成功.


**Git输入**： npm config get registry
 
**Git输入**： npm info express


- 通过cnpm使用

**Git输入**：npm install -g cnpm --registry=https://registry.npm.taobao.org

**Git输入**：cnpm install express


