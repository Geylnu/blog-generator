---
title: 使用Hexo+Github Pages给自己搭建一个博客
date: 2018-03-22 09:46:51
tags:
---
# 前言
由于我专业原因，有大量软件都必须使用Windows,并且我的笔记本在安装Linux的过程中，总容易出现各种兼容问题，因此没有安装Windows/Linux双系统，而在Windows上使用虚拟机也不是一个太棒的选择，最后使用了win10的Linux子系统，体验感觉还行，如果在Windows平台安装失败的同学可以参照我博客的Linux部分尝试安装。

# 在windows上安装
## 准备安装的必要工具
本篇默认使用**git bash**作为bash环境，并且自带**git**，可以从[这里](https://git-scm.com/download/win)下载安装，还需要安装**node.js**，可以从[这里](https://nodejs.org/zh-cn/)下载安装（记得勾选**Add to PATH**）。
我当前使用的node.js版本为v8.10.0，npm版本为5.6.0，版本不一致可能带来**不可预知的错误**。

## **进行本地安装**
1. 首先使用**npm**安装**hexo**
``` bash
$ npm install -g hexo-cli
```
你可以使用`hexo -v`查看是否安装成功

2. **创建的博客**
使用`cd`跳转到一个安全的目录，例如
``` bash
$ cd ~/Documents
```
    初始化你的博客
    ``` bash
    $ hexo init myblog
    ```
    * **这条命令执行中经常出现各种问题，可以`rm -rf myblog`删除这个目录重试**
    * **如果经常连接github失败，建议使用系统代理**

    * **仍然无法正确安装并初始化？**
    **可以用Linux子系统试试，参照本篇Linux部分**

3. **验证**
``` bash
$ cd myblog
$ npm i
```
    博客初始化就完成了

    现在输入
``` bash
$ hexo server
```
    然后在浏览器中访问`http://localhost:4000/`即可预览界面。
界面是这样的
![预览图像](look.png)

## 使用Github Pages
在Github上新建一个名为`你github用户名.github.io`的新repo
在博客目录中打开配置文件
``` bash
$ vim _config.yml
```
把最后一行`type `改成`type: git`
在最后一行，与type平齐，加上`repo: 仓库地址`
比如的仓库地址是：`git@github.com:Geylnu/Geylnu.github.io.git`
安装git部署插件
``` bash
npm install hexo-deployer-git --save
```
请在此前确认已经正确配置git
然后输入
``` bash
$ hexo deploy
```
OK
等上几分钟，访问`用户名.github.io`就能看见自己的博客了。
* 如果访问失败，检查Github Pages是否开启
* 如果缺少样式，检查仓库名是否正确

添加新的博客命令：
``` bash
hexo start "新的博客名"
```
重新生成页面使用
``` bash
hexo g
```
## 使用Coding Pages服务
由于总所周知的原因，Github在国内的访问很不稳定，由Github Pages支撑的博客自然也难以访问，[coding](https://coding.net)位于国内，并且也支持Pages服务。
使用coding时，只需要将仓库名命名为`你的用户名.coding.me`，就可以了，其他操作和Giihub pages相同。


# 在Linux(Ubuntu)上安装
## 我的系统环境
我使用的是**win10 1709版本**，os内部版本为**16299.309**，安装的Linux子系统为**Ubuntu 16.04.3**内核版本**4.4.0-43-Microsoft**
你可以[参照这里](https://www.jianshu.com/p/bc38ed12da1d)安装win10 Linux子系统。

## 安装node.js
使用官方的脚本安装
``` bash
$ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
$ sudo apt-get install -y nodejs
```
如果你是其他Linux发行版本或想安装其他版本，你可点此访问[相关说明](https://github.com/nodesource/distributions)

## 安装Hexo
使用npm安装hexo
``` bash
$ sudo npm install -g hexo-cli
```
安装完成后可以使用`node -v`和`npm -v`确认是否安装成功

**初始化博客**
``` bash
$ hexo init myblog
```
我在这一步总是遇见报错
![报错](err1.png)
根据错误提示尝试了各种办法都没有解决（还害的我重新装了次子系统），之后我在Github也看见了有人遇见了同样的[问题](https://github.com/npm/npm/issues/20103 "Rename Issues")

因为这个问题是偶发性的，其实直接`cd ./myblog`重新执行`npm install`

还是报错？
![报错](err2.png)
再来，直到不报错为止
![成功](suc.png)
OK
可以用`hexo s`验证是否能正确浏览,其他的就很在windows一样了









