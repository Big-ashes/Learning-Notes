# FRP 的配置和使用
FRP（fast reverse proxy）可以用于反向代理和内网穿透，支持TCP，UDP，HTTP，HTTPS，这样就可以将内网的服务非常便携的通过具有公网IP节点的形式暴露到公网，作者是一个中国人，并且在[GitHub上可以找到对应的项目](https://github.com/fatedier/frp)
> 这里指明[frp的官方文档](https://gofrp.org)

## 远程桌面

首先想到的就是远程桌面，配置frp，电脑作为客户端，公网服务器作为服务器端，电脑暴露共享桌面端口，可以自行设置，一般都是3389
![image-20220119210907525](https://cdn.jsdelivr.net/gh/Big-ashes/mypic@master/img/202201192109577.png)
然后分别设置客户端的frpc.ini 和服务器端的frps.ini，将相应的端口开放通行
通过RDclient就可以直接远程访问电脑，直接操作电脑，之前是在局域网内，非常不方便，换网就连不上了，但是拥有服务器之后，就可以把服务映射部署在公网上，任何时候都可以达到远程访问的目的，当然前提是你的客户端得开机。

### 客户端配置

需要在Windows端配置frp服务器端，还需要在任务计划程序中设置相对应的脚本开机启动

![image-20220119202520707](https://cdn.jsdelivr.net/gh/Big-ashes/mypic@master/img/202201192025470.png)
配置相应的端口映射端口服务，如smb服务和tcp服务的远程桌面

#### rdp的远程桌面
```
[common]
server_addr = XX.XX.XX.XX
server_port = XXXX
token = XXXXXXXXXXXX
```
```
[rdp]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 3389
```
```
[smb]
type = tcp
local_ip = 127.0.0.1
local_port = 445
remote_port = 7002
```
```
[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000
```


### 服务器端配置

需要在公网服务器上配置
下载解压安装frp
随后修改frps.ini，还可以配置一个端口作为管理页面，如下

![image-20220119211416902](https://cdn.jsdelivr.net/gh/Big-ashes/mypic@master/img/202201192114938.png)


两者都配置完成后，都启用，即可在RD client中直接通过访问ip：端口的形式访问电脑

![RD client](https://cdn.jsdelivr.net/gh/Big-ashes/mypic@master/img/202201192103378.jpg)

### 可能出现的错误

Your session has ended because of an error – Error Code 0x4
1.可能是网站域名没有备案的缘故，换成公网ip
2.远程设置里勾上仅允许运行使用网络级别身份验证
3.防火墙设置里允许程序或功能通过防火墙里找到远程连接remote desktop 后面两勾要勾上。防火墙新增出站入站规则。准许对应端口号通过。安全性不高可以设置all
4.win+R键，输入services.msc 打开，找到三项：(remote desktop services)
(remote desktop services usermod portredirector)
(remote desktop configuration)，右键属性中启动类型设置为自动，然后手动启动服务。


## SMB服务
2019年苹果在WWDC宣布增加了对APFS格式硬盘和SMB文件共享的支持，SMB的服务支持，可以很好的打破苹果和windows系统之间的隔阂，完成一种曲线救国的方案，完成之后就可以在ipad上直接操作win上暴露给ipad的文件夹，保存编辑等一应俱全

### smb的ipad文件服务
需要在客户端也就是电脑开放相应的本地端口

![image-20220119212002635](https://cdn.jsdelivr.net/gh/Big-ashes/mypic@master/img/202201192120684.png)
开通成功后，即可在ipad文件服务器连接这个smb服务

> SMB(Server Message Block)是一个协议，用于web连接与服务端之间的信息沟通