# 搭建LNMP环境



## 准备环境

- 关闭防火墙
```
systemctl status firewalld       #查看当前防火墙状态
systemctl stop firewalld          #关闭当前防火墙  
systemctl disable firewalld      #永久关闭防火墙
```
更多内容请查看[firewalld官网](https://firewalld.org/)
- 关闭SELinux
```
getenforce        #查看当前SELinux状态
setenforce 0      #临时关闭SELinux
```
更多内容请查看[开启或关闭SELinux](https://help.aliyun.com/document_detail/157022.html#task-2385075/)

### 安装Nginx
- 普通http 极速安装
> 本篇为普通安装,搭建HTTP网站
> https 需要编译安装,详细看另一篇 [Nginx编译安装](https://bigashes.com)
```
yum -y install nginx   #安装nginx
nginx -v               #查看版本,有列出信息则安装成功
```
### 安装Mysql

    dnf -y install @mysql  #安装mysql
    mysql -V			   #查询版本,有列出信息则安装成功

### 安装PhP

```
dnf -y install epel-release   
dnf update epel-release			#添加并更新epel源
    
dnf clean all        
dnf makecache					#删除缓存无用的软件包并更新软件源

dnf module enable php:7.3		#启用php:7.3模块
dnf install php php-curl php-dom php-exif php-fileinfo php-fpm php-gd php-hash php-json php-mbstring php-mysqli php-openssl php-pcre php-xml libsodium						#安装php相应的模块
php -v							#查询版本,有列出信息则安装成功
```
## 配置环境

### 配置Nginx
```    
cat /etc/nginx/nginx.conf   # 查看Nginx配置文件路径
```

> Centos安装的nginx配置文件一般默认为这个位置,Ubuntu下的位置为/var/nginx/sites-enabled/default

```
cd /etc/nginx/conf.d
cp default.conf default.conf.bak		#备份默认配置文件
vim default.conf						#vim编辑默认配置文件
```


在location /{  }括号内修改

```
location / {
    #将该路径替换为您的网站根目录。
    root   /usr/share/nginx/html;
    #添加默认首页信息index.php。
    index  index.html index.htm index.php;
}
```


修改php$,去掉注释


```
location ~ \.php$ {
    root           /usr/share/nginx/html; #将该路径替换为您的网站根目录。
    fastcgi_pass   unix:/run/php-fpm/www.sock;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    include        fastcgi_params;:wq
}
```
> fastcgi_pass:是Nginx通过unix套接字与PHP-FPM建立联系，该配置与/etc/php-fpm.d/www.conf文件内的listen配置一致。
> fastcgi_param:将/scripts$fastcgi_script_name修改为$document_root$fastcgi_script_name。
include:Nginx调用fastcgi接口处理PHP请求。

编辑好后文件结构应该如下
![nginx.conf](https://cdn.jsdelivr.net/gh/chuiluhui/mypic@master/img/202201071526881.jpg)

```
systemctl start nginx					#启动Nginx
systemctl enable nginx					#开机启动Nginx
```
### 配置MySQL
```
systemctl enable --now mysqld
systemctl status mysqld
mysql_secure_installation
```
> ubuntu下先查询 /etc/mysql/debian.cnf的默认密码
```
mysql -u debian-sys-maint -p    # 换行后输入上述查到的密码

```
进入mysql 
```
use mysql;
flush privileges;
ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'XXXXX';  #XXXX是密码
flush privileges;
quit;
service mysql restart
mysql -u root -p    # 回车后输入自己修改后的密码即可
```

### 配置PHP

```
vi /etc/php-fpm.d/www.conf
```
```
vim /目录/phpinfo.php
```
## 测试访问
在网站的目录下新建一个php文件

    vim info.php
    <?php echo phpinfo(); ?>  #这是文件中输入的内容
然后在浏览器中访问这个文件（域名/info.php）若出现配置信息则代表php-fpm和nginx正常通信

