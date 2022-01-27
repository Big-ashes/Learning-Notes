# 初识Docker

Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。
Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。
容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

## Docker概述
### 简介

[Docker官网](https://www.docker.com/)
> 虚拟机和Docker相比，虽然两者采用的都是虚拟化技术，但是虚拟机是将整个电脑虚拟了出来，所以很笨重，而Docker属于容器技术，采用隔离的方式，只展现最核心的环境部分，小巧方便。

### 架构
Docker重要的三个概念：
- 镜像（Image）		相当于一个root文件系统。其上可以创建多个容器。
- 容器（Container）	  是镜像运行时的实体，可以被创建，启动停止删除...
- 仓库（Repository）  可以看成一个代码控制中心，保存镜像
> 仓库默认是国外的，国内访问可以配置镜像加速

![docker架构](https://cdn.jsdelivr.net/gh/chuiluhui/mypic@master/img/202201101621542.png)

### 作用
集成开发运维DevOps（Development和Operations的组合词）必备，配置环境太麻烦了，运行一分钟，配置环境一天，docker核心思想就是隔离，打包装箱，集装箱的效果使交叉的应用分开。

### 学习
在自己学习的时候一定要注意看[Docker文档](https://docs.docker.com/)
还有仓库[Docker Hub](https://hub.docker.com/)，和Git仓库差不多，提供了数量庞大的镜像。

## Docker安装
Docker依赖已存在并运行的Linux内核环境，实质上是在已运行Linux的基础上创造了一个隔离的文件环境，执行效率几乎等同于Linux主机，因此，Docker必须部署在Linux内核的系统上，在其他系统部署时需要安装Linux环境。

- win
可以使用Docker-Desktop
> Windows有Hyper-V,它是微软开发的，仅适用于windows，可以在应用和功能界面开启

- centos
1. 卸载旧版本
```
 yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
2. 安装软件包
```
yum install -y yum-utils				#提供实用程序
``` 				
3. 设置库
```
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo		#国外下载地址，建议国内
    
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo  #用阿里云镜像地址
```
4. 安装docker
安装时选择社区版本（docker-ce）  ee是企业版
``` 
yum install docker-ce docker-ce-cli containerd.io	#安装docker引擎
```
>还可以用脚本
>```
>curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun  #官方脚本
>curl -sSL https://get.daocloud.io/docker | sh			#国内daocloud一键安装
>```

### 启动
启动程序并跑一个hello-world测试
```
systemctl start docker
docker run hello-world
```

![Hello-world](https://cdn.jsdelivr.net/gh/chuiluhui/mypic@master/img/202201101726970.png)
> 从上可以看出，hello-world 的镜像是下载下来的，可以查看一下
>```
>docker images
>```

卸载Docker时，用yum remove卸载依赖，其次使用rm-rf删除相关文件以及资源
```
yum remove docker-ce docker-ce-cli containerd.io
rm-rf /var/lib/docker										#Docker默认工作路径
```