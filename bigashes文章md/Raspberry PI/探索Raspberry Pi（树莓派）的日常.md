# 探索Raspberry Pi(树莓派)日常

## RaspberryPi GUI系统初始配置和换源
有了树莓派可以做什么？
2020疫情期间，因呆在家里无所事事，遂购入树莓派一只，开始了探索之旅。
此篇文章导图

![raspberry](https://cdn.jsdelivr.net/gh/chuiluhui/mypic@master/img/202201071602255.png)

进入[树莓派官网](https://www.raspberrypi.org/)可以查看到可下载的相关系统，不同的系统对应不同的功能（可以多做折腾和研究）

### 系统安装

初学的我，这里我选择了树莓派官网的Rasberry pi GUI操作系统

下载之后解压发现是img文件，通过[balenaEtcher](https://www.balena.io/etcher/)安装到tf卡中。

![2](https://cdn.jsdelivr.net/gh/chuiluhui/mypic@master/img/202201071603314.jpg)

> 这里需要提前将SSH和网络的配置文件丢入tf卡，方便后期的操作。
具体有两个文件：

1.在boot盘更目录下构建ssh文件（无后缀无内容）可以通过txt更改文件名做到。
2.wpa_supplicant.conf(这个文件是默认的联网配置文件)：
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=CN
network={
    ssid="1234567"  #WiFi账号
    psk="123456789"  #WiFi密码
}
```
可以填写默认连接的wifi （也可以有GUI界面可以进系统后再考虑）

### SSH和换源
树莓派的SSH功能和网络配置在上一步进行了操作，


换源:树莓派自带nano编辑器，因此可直接访问配置文件，我们需要更换软件源和系统源：（这里换成清华镜像源）

    sudo nano /etc/apt/sources.list
清华源:
```
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
```
进入到界面之后，注释掉原来的源，换上新的数据源：

![list](https://cdn.jsdelivr.net/gh/chuiluhui/mypic@master/img/202201071607678.png)

>这里注意一下数据源的版本问题，如果采用老版数据源的话，后续更新软件会出现问题。

数据源还没换完，还需要换这个源： 

sudo nano /etc/apt/sources.list.d/raspi.list

还是上面的源，注释原来的，加上要换的。

### 配置个性化
可以配置conky,配置界面如下:

![conky](https://cdn.jsdelivr.net/gh/chuiluhui/mypic@master/img/202201071556591.jpg)

到这里，树莓派的基本配置就大功告成啦~

 
