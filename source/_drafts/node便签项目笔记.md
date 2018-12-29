---
layout: _draft
title: node便签项目笔记
date: 2018-07-14 17:30:18
tags:
---

# 初始化
* git仓库初始化
* npm init 初始化
* 安装express `npm install express --save`
    `--save`标识将该包添加入依赖，会写入package.json文件中
    如果遇到问题可以使用npm安装nrm使用镜像源
* git 添加.gitignore
* 网页需要的东西很多，自己一个个添加很麻烦，所以可以用现成的脚手架express-generator来生成
* `npm install express-generator --save-dev` 安装脚手架 除非自己电脑，不应该使用`-g`全局安装
-dev参数作用：只在开发时依赖,运行时不会使用
* 使用指定运行`./node_modules/express-generator/bin/express-cli.js`
    * -f 强制使用当前目录而不是新建一个目录 
    * -e 使用ejs模板引擎

* npm install 安装所有依赖

# 生成文件解读
## app.js 
MVC 
M: 数据库
V:模板引擎
C:路由

express大量使用中间件
中间间可以看官网

一些点: res.send()之后就不再执行了
app.use 所有请求方式
app.get 单个请求方式
经过路由后，传给下级路由的是后面的路由参数

## 模板引擎
特殊的符号标识数据，解析替换

## 路由
优先匹配静态资源，再向下一级

# 启动
可以通过npm start 启动（其实执行的是也是下面这个）
一般方法是直接启动 node ./bin/www启动
可以在前添加PORT=1000指定启动端口,我们一般没有sudo权限，也就不能使用80端口

# 前端静态资源
开发使用的东西会和实际的代码不同，比如用了webpack生成的，所以我们可以创建一个src目录

src目录的js也可能需要引用一些其他库，我们可以创建一个libs来存这些，也可以直接安装进node_models，使用webpack来分离

自己再建一个apps目录，代表不同目录的js

再新建一个mods，自己通用的模块

可以按功能来划分和应用来划分

webpack.config.js如果只有前端用，就放到src目录

安装webpack 非自己电脑可以用--save-dev 自己电脑直接用全局吧

然后配置（照着抄）
ok

但是我们每次更改后都需要重新打包进行编译，这很不方便，我们可以用onchang

注意 onchange中路径必须使用双引号，因此需要使用转义符

# less
``` less
@color: #ffffff
.rounded-corners(@xxx:5px) //5px为默认值
```

# webpack
名称重定向
``` 
resolve: {
    alias:{
        jquery: path.joni(_dirname,"./sxbbxbx")
    }
}
```
这有更高的优先级，有的话就不会去node_modules

插件
``` js
plugins:{
    new webpack.ProvidePlugin({
        $: "jquery"
    })
}
```
引入
``` js
require(less/xxx.less)
```

事件采用发布订阅模式
每个组件模块化，最后由一个总控制器调用api

## 多级路由
### 页面URL
每个进行分类
###接口URL 
/以api开头，方便识别这是一个接口
创建
修改
删除
查询

rest风格， 
使用状态码，告诉失败原因（错误返回码）

不懂js可以用sequelize,对数据库接口的封装

调试工具 node-inspector
默认占用8080端口
调试只需要在启动参数前面加上 --debug

使用path更安全

第三方登陆
跳转到第三方，登陆后向原域名服务器提交相关信息并跳转
权限 管理员 用户私人 OAuth协议
passport做认证的
整个流程 用户带点击 服务器得知，向带着自己的id向服务器做认证请求 ，同时回调

登陆后操作数据仍然需要用户uid。否则任何登陆用户都能删除其它用户的数据

登陆后展示自己的便签