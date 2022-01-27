# Docker配置

Docker配置镜像加速
在阿里云选择镜像加速器
修改daemon配置文件 /etc/docker/daemon.json
```
mkdir -p /etc/docker

tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://snaftssc.mirror.aliyuncs.com"]
}
EOF

systemctl daemon-reload
systemctl restart docker
```