# Nginx踩坑之路

这里稍微详细叙述下Nginx的原理和一些可能遇到的问题，都是bigash本人摸索出来的，如有错误，欢迎指正

## Nginx 原理
查阅资料后bigash自己得到的理解，大部分都是和搭建个人网站时遇到的问题挂钩

### Nginx

> nginx常用命令
> ps -ef | grep nginx       #检查nginx进程
> 一般来讲有两个进程 worker和master 
> master 主进程   通过接受信号发给worker
> worker 工作进程  更改在nginx.conf

更改事件在event下增加，默认使用的是epoll worker_connections 每个worker默认连接的最大连接数
##  Nginx.conf配置
> main 全局配置
> > event 配置工作模式以及连接数
> > > http  http模块相关配置
> > > > server  虚拟主机配置 可多个
> > > > > location 路由测试 表达式   匹配规则 正则表达式
> > > > > upsteam 集群 内网服务器

***

###  Nginx和php

####  Nginx和php的连接问题
> nginx和php不能通信 
> 文件不存在
> php-fpm和nginx不能通信
> root位置错误

## Nginx日常使用记录
### 查看进程
```
ps -ef | grep nginx       					#检查nginx进程
```
> 一般来讲有两个进程 worker和master 
- master 主进程   通过接受信号发给worker
- worker 工作进程  更改在nginx.conf

### 查看错误日志
```
var/lof/nginx/error.log						#nginx错误日志，可用cat查看
ll   										#查看目录权限，权限不足可能有php-fpm和nginx无法通信的情况
```
### 修改权限 
```
chmod 777 xxx								#给予xxx最高权限   chmod 权限数字 文件名 
```
