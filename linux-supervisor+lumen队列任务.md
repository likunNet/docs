# docs
杂项（服务器、数据库等相关安装配置文档）
CentOS7安装Supervisor3.1.4
supervisord

负责管理进程的server端，配置文件是/etc/supervisor/supervisord.conf

supervisorctl

client端的命令行工具，管理子进程，配置文件在/etc/supervisor/supervisord.d/目录下

 

yum install -y supervisor

安装supervisor

（如果用的是阿里云的CentOS7会提示找不到supervisor，

则yum install epel-release先安装EPEL源）

 
开机自启
systemctl enable supervisord
 
启动supervisord
systemctl start supervisord

查看状态
systemctl status supervisord


创建supervisor所需目录

# mkdir /etc/supervisord.d/
1
创建supervisor配置文件

# echo_supervisord_conf > /etc/supervisord.conf
1
编辑supervisord.conf文件

# vim /etc/supervisord.conf
