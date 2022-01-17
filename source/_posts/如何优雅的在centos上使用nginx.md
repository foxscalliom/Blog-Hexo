---
title: 如何优雅的在centos上使用nginx
tags:
  - centos
  - nginx
categories:
  - 前端
abbrlink: 27f7e408
date: 2022-01-12 01:08:19
---
### 启动nginx服务
#### nginx直接启动
在CentOS7.4版本里，直接直接使用nginx启动服务的。
```sh
nginx
```
<!--more-->
#### 使用systemctl命令启动
还可以使用个Linux的命令进行启动，我一般都是采用这种方法进行使用。因为这种方法无论启动什么服务，都是一样的，只是换一下服务的名字（不用增加额外的记忆点）。
```sh
systemctl start nginx.service
```
输入命令后，没有任何提示，那我们如何知道Nginx服务已经启动了哪？可以使用Linux的组合命令，进行查询服务的运行状况。
```sh
ps aux | grep nginx
```
如果有三条记录，说明Nginx被正常开启了。
### 停止nginx服务的四种方法
停止Nginx 方法有很多种，可以根据需求采用不一样的方法，一个一个说明。
#### 立即停止服务
```sh
nginx -s stop
```
这种方法比较强硬，无论进程是否在工作，都直接停止进程。
#### 从容停止服务
```sh
nginx -s quit
```
这种方法较stop相比就比较温和一些了，需要进程完成当前工作后再停止。
#### killall方法杀死进程
```sh
killall nginx
```
这种方法也是比较野蛮的，我们直接杀死进程，但是在上面使用没有效果时，我们用这种方法还是比较好的。
#### systemctl停止服务
```sh
systemctl stop nginx.service
```
### 重启nginx服务
有时候要重启nginx服务，可以使用下面的命令
```sh
systemctl restart nginx.service
```
### 重新载入配置文件
在重新编写或者修改Nginx的配置文件后，都需要作一下重新载入，这时候可以用Nginx给的命令。
```sh
nginx -s reload
```