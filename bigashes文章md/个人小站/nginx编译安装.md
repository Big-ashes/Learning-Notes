# 安装Nginx

+ 编译安装  编译安装是自己下载源码包，解压安装，可以指定配置参数，兼容性更强
+ 极速安装  在线安装 如yum install 下载安装包 自动分析所需的软件依赖关系，简单方便，无需考虑依赖
这篇记录的是编译安装，环境是Centos8

---
## 下载nginx
```
groupadd -r nginx
useradd -r -g nginx nginx						#添加nginx服务进程的用户
wget http://nginx.org/download/nginx-1.20.0.tar.gz			#下载nginx压缩包
tar xvf nginx-1.20.0.tar.gz -C /usr/local/src	#解压到usr/local/src目录下
```
这里下载的位置可以新建一个软件文件夹，按照本人的习惯，软件包都放在根目录home/software下
接下来安装编译工具，进行编译安装
## 准备编译工具
```
yum groupinstall "Development tools"		#下载安装编译工具
yum -y install gcc wget gcc-c++ automake autoconf libtool libxml2-devel libxslt-devel perl-devel perl-ExtUtils-Embed pcre-devel openssl-devel
cd /usr/local/src/nginx-1.20.0
```
还可以这样一步一步来，不需要很多环境，建议按照第一种
```
yum install gcc-c++   						#安装gcc环境
yum install -y pcre pcre-devel 				#安装pcre库 用于解析正则表达式
yum install -y zlib zlib-devel 				#安装压缩和解压缩依赖
yum update
yum install -y openssl openssl-devel 
#ssl加密的套接字协议层，用于http安全传输，也就是https
```
## 编译nginx

```
./configure \
--prefix=/usr/local/nginx \
--sbin-path=/usr/sbin/nginx \
--conf-path=/etc/nginx/nginx.conf \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--pid-path=/var/run/nginx.pid \
--lock-path=/var/run/nginx.lock \
--http-client-body-temp-path=/var/tmp/nginx/client \
--http-proxy-temp-path=/var/tmp/nginx/proxy \
--http-fastcgi-temp-path=/var/tmp/nginx/fcgi \
--http-uwsgi-temp-path=/var/tmp/nginx/uwsgi \
--http-scgi-temp-path=/var/tmp/nginx/scgi \
--user=nginx \
--group=nginx \
--with-pcre \
--with-http_v2_module \
--with-http_ssl_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_sub_module \
--with-http_dav_module \
--with-http_flv_module \
--with-http_mp4_module \
--with-http_gunzip_module \
--with-http_gzip_static_module \
--with-http_random_index_module \
--with-http_secure_link_module \
--with-http_stub_status_module \
--with-http_auth_request_module \
--with-mail \
--with-mail_ssl_module \
--with-file-aio \
--with-ipv6 \
--with-http_v2_module \
--with-threads \
--with-stream \
--with-stream_ssl_module						#编译的参数，具体可以查看官网

```
这里放出官网的参数说明：http://nginx.org/en/docs/configure.html
```
make  && make install
mkdir -p /var/tmp/nginx/client					#新建一个目录
```
## 添加SysV启动脚本
    vi /etc/init.d/nginx
```
#!/bin/sh 
# 
# nginx - this script starts and stops the nginx daemon 
# 
# chkconfig:   - 85 15 
# description: Nginx is an HTTP(S) server, HTTP(S) reverse \ 
#               proxy and IMAP/POP3 proxy server 
# processname: nginx 
# config:      /etc/nginx/nginx.conf 
# config:      /etc/sysconfig/nginx 
# pidfile:     /var/run/nginx.pid 
# Source function library. 
. /etc/rc.d/init.d/functions
# Source networking configuration. 
. /etc/sysconfig/network
# Check that networking is up. 
[ "$NETWORKING" = "no" ] && exit 0
nginx="/usr/sbin/nginx"
prog=$(basename $nginx)
NGINX_CONF_FILE="/etc/nginx/nginx.conf"
[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx
lockfile=/var/lock/subsys/nginx
start() {
    [ -x $nginx ] || exit 5
    [ -f $NGINX_CONF_FILE ] || exit 6
    echo -n $"Starting $prog: " 
    daemon $nginx -c $NGINX_CONF_FILE
    retval=$?
    echo 
    [ $retval -eq 0 ] && touch $lockfile
    return $retval
}
stop() {
    echo -n $"Stopping $prog: " 
    killproc $prog -QUIT
    retval=$?
    echo 
    [ $retval -eq 0 ] && rm -f $lockfile
    return $retval
killall -9 nginx
}
restart() {
    configtest || return $?
    stop
    sleep 1
    start
}
reload() {
    configtest || return $?
    echo -n $"Reloading $prog: " 
    killproc $nginx -HUP
RETVAL=$?
    echo 
}
force_reload() {
    restart
}
configtest() {
$nginx -t -c $NGINX_CONF_FILE
}
rh_status() {
    status $prog
}
rh_status_q() {
    rh_status >/dev/null 2>&1
}
case "$1" in
    start)
        rh_status_q && exit 0
    $1
        ;;
    stop)
        rh_status_q || exit 0
        $1
        ;;
    restart|configtest)
        $1
        ;;
    reload)
        rh_status_q || exit 7
        $1
        ;;
    force-reload)
        force_reload
        ;;
    status)
        rh_status
        ;;
    condrestart|try-restart)
        rh_status_q || exit 0
            ;;
    *)
      echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}" 
        exit 2
esac
```
脚本可以将执行文件用更短的代码运行，不用每次都进入目录./sbin/nginx
```
chmod +x /etc/init.d/nginx						#为脚本添加权限
chkconfig --add nginx							#添加到服务管理列表
chkconfig  nginx on								#设置开机启动
systemctl start nginx							#启动nginx服务
```

这个时候就部署成功了，访问公网ip即可看到如下界面

![image-20220122144312287](https://cdn.jsdelivr.net/gh/Big-ashes/mypic@master/img/202201221443668.png)

