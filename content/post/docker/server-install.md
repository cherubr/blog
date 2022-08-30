---
title: "Server-Docker 相关配置"  
date: 2019-12-26T15:46:02+08:00  

categories: ["Docker"]
tags: ["nginx，mysql,redis环境配置"]
---
## Docker相关服务配置
- docker安装
- mysql安装
- redis安装
- nginx安装
- rabbitmq安装
## docker安装
- 移除现有的docker文件  
```linux
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
- 安装所需的软件包。yum-utils提供了yum-config-manager 效用，  
并device-mapper-persistent-data和lvm2由需要 devicemapper存储驱动程序  
```linux
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
- 设置存储仓库  
```linux
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
- 安装docker  
```linux
yum install docker-ce docker-ce-cli containerd.io
```

- docker镜像加速  
```linux
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://2u4lx3xv.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
```

## mysql docker 安装
- 创建对应目录给予对应文件权限
```docker
docker run --name mysql -p 3306:3306 -v /root/docker/mysql/conf.d:/etc/mysql/conf.d  -v /root/docker/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123123gg  -d mysql
```
- 登录mysql
```docker
docker -exec -it mysql /bin/bash  
mysql -u root -p123123gg
ALTER USER 'root'@'localhost' IDENTIFIED BY '123123gg';
```
- 赋予远程登录权限  
```mysql
CREATE USER 'test'@'%' IDENTIFIED WITH mysql_native_password BY '123123gg';
GRANT ALL PRIVILEGES ON *.* TO 'test'@'%';
```
## redis 安装（以配置文件方式启动）
- 创建对应目录给予对应文件权限，下载redis.conf文件
[redis.conf](http://download.redis.io/redis-stable/redis.conf) (linux 使用wget下载)
```docker
docker run -d \
-p 6379:6379 \
-v /root/docker/redis/config/redis.conf:/etc/redis/redis.conf \
-v /root/docker/redis/data:/data \
--privileged=true \
--name redis \
redis \
redis-server /etc/redis/redis.conf
```
- 对于redis redis.conf文件配置（阿里云里面应该设密码防止被挖矿）
```
#bind 127.0.0.1  远程链接
requirepass test123 设置密码
```
##  nginx 安装
```docker
docker run --name nginx \
-p 8080:80 \
-d \
-v /root/docker/nginx/conf.d/default.conf:/etc/nginx/conf.d/default.conf \
-v /root/docker/nginx/nginx.conf:/etc/nginx/nginx.conf \
-v /root/docker/nginx/logs:/var/log/nginx  \
-v /root/docker/nginx/html:/usr/share/nginx/html nginx
```
### docker cp （default.conf，nginx.conf文件夹）
保证当前cp的容器没有挂载该文件
进入容器：```docker exec -it nginx /bin/bash```
执行命令：``` nginx -t ```找到对应conf文件
### (注意挂载过程出现403问题)  
- 1.修改nginx.conf文件的 #user  nobody; ==》root或者nginx用户名
- 2.添加添加selinux规则，改变要挂载的目录的安全性文本
 ```linux 
 chcon -Rt svirt_sandbox_file_t /home/hct/sample/
```
- 3.html文件夹没有文件（添加一个index.html）

## rabbitmq
docker pull docker.io/rabbitmq:3.8-management  
docker run --name rabbitmq -d -p 15672:15672 -p 5672:5672 rabbitmq:3.8-management









