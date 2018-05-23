---
title: vultr服务器搭建ss心得
date: 2018-05-23 14:09:53
tags: Computer Networking
---

# vultr服务器下搭建shadowsocks
### 基于Ubuntu的代理服务器搭建

作为一个科班学生，我觉得自己学会搭建一个服务器是必要操作。虽然搭好了也没有多大用处，墙外的世界也没有什么好看的，英文不好的我确实觉得没什么好看的。hhhhhh

首先，服务器我用的是Vultr的Ubuntu最新版，最便宜的是5刀一个月。限流量但是绝对够用了。在服务器上直接配置操作不方便，可以下载一个Xshell来远程控制服务器，在本地直接进行操作。[Xshell下载地址](https://www.netsarang.com/products/xsh_overview.htm)

Xshell操作很简单的，新建会话然后把服务器那边给你的IP和账号（vultr那里账号默认是root）和密码填一下，其他的默认，就可以用了。

进入Ubuntu之后，都是命令行操作。我很呆，我装了一个Ubuntu界面，然后发现下载完shadowsocks之后找不到文件...之后才明白我的安装方式本来就没有装图形界面，要有图形界面得另外一种安装方式，其实也没有必要，没有就没有吧，熟悉一下命令行操作不好吗？

首先下载各种需要的东西：
* 需要一个pip环境（此处用的是python版本的shadowsocks，这里提一下lievb，这个是大多数人比较支持的，投票赞同率有60%多，python版本的有30%多，GO版本的有8%，因为lievb内存占用小，速度应该二者差不多。感觉python版本的更好部署一点，我秉着实现优先的原则，就先用pytho版本的试一下）
```
sudo apt-get install python-pip
```
* 安装pip之后就可以用pip来装shadowsocks了。
```
sudo pip install shadowsocks
```
* 好了，现在可以使用服务器端的shadowsocks了，有两种方法来部署。
### 第一种方法：直接命令启动
```
sudo ssserver -p 8388 -k password -m -aes-256-cfb -d start
sudo ssserver -p 8388 -k password -m -aes-256-cfb -d stop
```
两行命令分别是启动和关闭，8388是端口，取决于你想开什么端口，-k后面是密码，-m后面是加密方式，我采用的是aes-256-cfb，这个比较安全，但是会慢一些？

然后
```
nohop ssserver
```
就可以让它在后台运行了。

### 第二种方法：配置json文件（这种方式虽然麻烦一些，但却方便查看和修改）
先切换个目录，默认是etc，在里面用vim指令新建一个json文件，然后配置那个json文件就行
```
cd /etc
vim shadowsocks.json
```
按回车进入json文件，按i进行insert操作：
```
{
    "server":"0.0.0.0",
    "port_password":{
            "123":"password",
            "10000":"password",
            "53":"password"
    },
    "local_address":"127.0.0.1",
    "local_port":1080,
    "timeout":60,
    "method":"aes-256-cfb"
}
```
配置完成之后按Esc退出，**然后输入:wq保存json文件并退出。**

接下来输入
```
sudo ssserver -c /etc/shadowsocks.json -d start
nohop ssserver
```
就可以了，这时候关闭Xshell，在本地打开客户端shadowsocks尝试一下能否连接服务器。
在此之前，先使用telnet试试服务器那边的端口开了没
```
telnet server_ip server_port
```
在客户端使用shadowsocks，填入目标ipv4地址，端口号和密码，以及加密方式，点击确定，然后启动系统代理。可以在帮助那栏看到显示日志，日志上会有一个started。那就是连接成功了！