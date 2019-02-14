---
title: express简单入门
tags:
- express
---
# express是什么？
官方介绍：`Express 是一个基于 Node.js 平台的极简、灵活的 web 应用开发框架，可以轻松的创建各种 web 或者移动端应用`

# 安装
## 从零开始
1. 建立一个工作目录
``` bash
mkdir myapp
cd myapp
```
2. 初始化配置文件
``` bash
npm init
```
配置过程中注意设置入口文件
```
entry point: (index.js)
```
输入app.js
3. 安装express
``` bash
npm install express --save
```
将express添加进依赖列表

## 使用脚手架工具
1. 安装脚手架工具
``` bash
npm install express-generator -g
```
`-h`可以列出所有帮助选项
2. 构建
``` bash
express --view=ejs myapp
```
指定ejs为视图引擎创建myapp

安装依赖
``` bash
cd myapp
npm install
```

测试启动
``` bash
DEBUG=myapp:* npm start
```
其中前者为设置全局变量来指定启用调试，输入日志

# 使用
## 静态文件
express使用内部的中间件函数`express.static`处理静态文件
``` js
app.use(express.static('public'));
```
可以指定多个目录
``` js
app.use(express.static('public'));
app.use(express.static('files'));
```
这种指定目录的方式不会在访问路径中体现，这里默认是以根目录的方式访问

如果想设置虚拟路径，可以重新使用路由
``` js
app.use('/static', express.static('public'));
```

**值得注意的是**向`express.static`函数提供的路径相对于您在其中启动 node 进程的目录，如果从另一个目录运行 Express 应用程序，那么对于提供资源的目录使用绝对路径会更安全。
``` js
app.use('/static', express.static(__dirname + '/public'));

```