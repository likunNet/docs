# docs
杂项（服务器、数据库等相关安装配置文档）
linux  docker安装 （CentOS8）

设置yum仓库
安装必要依赖包

$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
添加阿里镜像稳定版仓库

$ sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
添加阿里源时有时会报错，如果报错，使用如下命令使用官方源

#删除异常源
sudo rm -f /etc/yum.repos.d/docker-ce.repo
#使用官方源
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
更新yum缓存
sudo yum makecache fast
安装Docker-CE
注意事项：本步骤分两部分，仅需按需求使用其一

1 安装最新版
sudo yum install -y docker-ce docker-ce-cli containerd.io
2 安装指定版本
列出可用版本

$ yum list docker-ce --showduplicates | sort -r

docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable
安装指定版本

<VERSION_STRING>需要替换为第二列的版本号，如：18.06.0.ce-3.el7

$ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
启动Docker服务
sudo systemctl start docker




centos8默认使用podman代替docker，所以需要containerd.io，那我们就安装一下就好了

4.解决方法
安装containerd.io即可

yum install https://download.docker.com/linux/fedora/30/x86_64/stable/Packages/containerd.io-1.2.6-3.3.fc30.x86_64.rpm




删除image
docker rmi -f image_ID 
##单个镜像删除，相当于：docker rmi redis:latest
docker rmi redis
##强制删除(针对基于镜像有运行的容器进程)
docker rmi -f redis
##多个镜像删除，不同镜像间以空格间隔
docker rmi -f redis tomcat nginx
##删除本地全部镜像
docker rmi -f $(docker images -q)

## 容器重命名
docker rename compassionate_boyd nginx-dk
##停止一个运行中的容器
docker stop redis
##杀掉一个运行中的容器
docker kill redis
##删除一个已停止的容器
docker rm redis
##删除一个运行中的容器
docker rm -f redis

安装 nginx
docker pull nginx
首先在宿主机创建要挂载的目录
mkdir -p /data/docker  
mkdir -p /data/docker/nginx/conf #存放配置文件


安装完成后，我们可以使用以下命令来运行 nginx 容器：

$ docker run --name nginx-test -p 8080:80 -d nginx
参数说明：

--name nginx-test：容器名称。
-p 8080:80： 端口进行映射，将本地 8080 端口映射到容器内部的 80 端口。
-d nginx： 设置容器在在后台一直运行。



 -- 挂载本地目录
docker run --privileged -it -p 80:80 \
-v /data/docker/nginx/conf/nginx.conf:/etc/nginx/nginx.conf:ro \
-v /data/docker/nginx/conf/conf.d:/etc/nginx/conf.d:ro \
-v /data/docker/nginx/html:/usr/share/nginx/html:rw \
-v/data/docker/nginx/logs:/var/log/nginx -d nginx










docker快速搭建php7.2-nginx开发环境
1.输入命令：

docker search -s 100 php

搜索出下面图中列表，选择webdevops/php-nginx。

通过docker拉取webdevops/php-nginx镜像，我选择的最新的。

docker pull webdevops/php-nginx

3.通过docker创建一个容器：

docker run -itd --name php-ngins-wjs1 -p 89:80 -v /var/www/:/app/ webdevops/php-nginx
命令解释：

-i: 以交互模式运行容器，通常与 -t 同时使用

-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；

-d: 后台运行容器，并返回容器ID；

--name="nginx-lb": 为容器指定一个名称；

-p: 指定端口映射，格式为：主机(宿主)端口:容器端口，我把容器nginx的80端口映射到我的本地的89端口

-v: 绑定一个卷，把本地的/var/www 目录绑定到容器的 /app 目录

