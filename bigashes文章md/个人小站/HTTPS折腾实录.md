# HTTPS折腾实录

给自己网站增加小绿锁
- 申请免费证书
> 阿里云，腾讯云等

-  用Lets's Encrypt
> [Lets's Encrypt](https://letsencrypt.org/)这是一个免费、开放、自动化的证书颁发机构。

- 配置一个自签名的SSL证书
> [过程](https://www.liaoxuefeng.com/article/990311924891552)

## 申请免费证书

1. 可以直接进入各大服务器运营商的网站寻找免费SSL，然后申请
2. 在自己的域名管理界面录入解析
3. 下载合适的证书（Nginx，Apache，Tomcat.....）
4. 在服务端配置（nginx下配置nginx.conf,但是需要注意nginx下必须有ssl模块）

申请完成后进行配置
### 上传密钥
上传之后进行配置，配置nginx.conf文件
### 配置文件
这里配置的时候需要检查Nginx是否有ssl模块，如果没有需要重新编译安装
### 可能遇到的问题
https访问静态页面成功，即https配置成功，但是动态页面访问不了，或者是css样式加载不出来都是Nginx与php-fom的通信配置出错引起的，耐心排查

## Lets's Encrypt
- 也可以进入[免费https](https://certbot.eff.org/)项目主页选择后进行配置

这里直接在服务器端进行配置，需要注意以下三点
- 域名成功解析到服务器ip，80端口开放
- 有指向公网IP地址的A记录
```
apt-get update     #获取更新目录
apt-get upgrade    #更新目前的包
apt-get install software-properties-common
add-apt-repository ppa:certbot/certbot
apt-get update
apt-get install python3-certbot-nginx  #python-cert-nginx被替换成这个了
```
安装Nginx版本证书
```
cert --nginx
```
> 需要输入邮箱以及域名信息

### 设置自动更新脚本

    certbot renew --dry-run  # 测试一下是否正常
写一个update.sh脚本

```
#!/bin/bash
last_run_time=0
date1=date +%s
interval_days_secs=$((87*24*3600))
if [[ $((date1 - last_run_time)) -gt $interval_days_secs ]]; then
	certbot renew
    sed -i '2 s/[0-9][0-9]*/'$date1'/' update.sh
fi
```

```
0 0 * * * /root/update.sh > /root/log 2>&1		#新建一个任务
```

```
/etc/init.d/cron restart				#重启crontab服务
```



### 删除和撤销域名
    certbot revoke --cert-path /etc/letsencrypt/archive/XXXXX.com/cert1.pem  #撤销XXXXX.com配置的证书
    certbot delete    #删除证书
> 需要注意的是删除完之后需要更改配置文件中Certbot自动增加的配置