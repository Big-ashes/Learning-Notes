# Linux常用的一些操作

bigash常用的一些基础命令

## 换源

### Ubuntu

```
etc/apt/sources.list     					#进入目录后gedit或vim编辑
```
### Centos
```
/etc/yum.repos.d/CentOS-Base.repo           #找到目录后直接下载新的CentOS-Base.repo文件 
```
> 发现主流的云服务器厂商的VPS中源都是自家的

## 查看系统和CPU
```
uname -a            		#都可用
lsb_release -a      		#列出详细版本信息  centos redhat debian...
cat /proc/cpuinfo  			#查看系统的cpu相关信息
cat /etc/os-realease		#系统版本
```
## 查看当前用户信息和组信息
```
cat /etc/passwd
cat /etc/group
groups
whoami
groups test
```
## 查找文件所在位置
```
pwd						   #查看当前所属的目录路径
find    				   #根据文件属性查找
find / -name nginx.conf    #根目录起查找nginx.conf文件
which    				   #查看可执行文件所在位置
whereis   				   #查看特定文件，只用于二进制文件，源代码文件和man手册页
locate   				   #配合数据库查看文件位置，需要下载
```
## 卸载删除重装
### Ubuntu
```
apt-get unistall
apkg --list 								#列出安装的所有软件列表 /查找
apt-get --purge remove XXXX 				#加了--purge是删除包和配置文件
```
### Centos
```
yum remove XXXX
yum list installed
```
## 进程控制

## 查看系统运行情况
```
top											#展示系统运行情况
htop										#展示服务器负载情况（debian）
```
