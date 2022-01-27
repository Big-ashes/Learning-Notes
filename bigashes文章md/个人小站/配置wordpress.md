# 配置Wordpress

## 数据库配置

    mysql -uroot -p
进入数据库后输入以下命令

```
create datebase wordpress;					#创建数据库wordpress
use wordpress								#到wordpress数据库下操作
create user 'wordpress'@'localhost' identified by 'XXXXXX';     #创建用户wordpress密码是XXXXXX
grant all privileges on wordpress.* to 'wordpress'@'localhost';  #给予wordpress所有权限
flush privileges;							#刷新权限
exit;										#退出数据库
```
## wordpress配置文件
下载好wordpress文件后解压
```
cd /XXXXX/wordpress							#进入wordpress文件夹
cp wp-config-example.php wp-config.php		#复制默认配置文件进行配置
```

键入自己刚才设置的数据库名称，用户名，以及密码。
根据文档中的连接配置唯一的码，（好像并不影响）

还需要在wp-config.php加入
```
define("FS_METHOD", "direct");
define("FS_CHMOD_DIR", 0777);
define("FS_CHMOD_FILE", 0777);
```

## 修改nginx配置文件

相当于修改Nginx中配置的默认网页目录，指到Wordpress即可
修改nginx.conf 或者 sites-enabled下的default
> 也可以直接把文件复制过去
```
cp -r /root/wordpress/* /var/www/html    #复制
chown -R www-data:www-data /var/www/html  #修复权限
```
完成后重启Nginx

## 访问域名 
访问wordpress下的index.php 进入安装界面

## 配置wordperess站点



ftp的权限
```
chown -R apache:root /usr/share/wordpress
```


### 默认上传大小

安装好网站之后需要配置主题，上传文件等，这里就需要修改一下wordpress的默认配置了

打开php.ini，把 upload_max_filesize 和 post_max_size 修改为50M，然后重启。
    service php7.4-fpm restart
再次上传，问题依旧的话，可以排除php方面的问题，转而修改nginx.conf调整默认上传大小即可