# docs
杂项（服务器、数据库等相关安装配置文档）
【配置PHP】

在之前编译的源码包中，找到 php.ini-production，复制到/usr/local/php下，并改名为php.ini：

  cp php.ini-production /usr/local/php/php.ini
[可选项] 设置让PHP错误信息打印在页面上 

  vim /usr/local/php/php.ini 
  display_errors = On

复制启动脚本：

  cp ./sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
 
  chmod +x /etc/init.d/php-fpm
修改php-fpm配置文件：

  cd /usr/local/php/etc
 
  cp php-fpm.conf.default php-fpm.conf
 
  vim php-fpm.conf
① 去掉 pid = run/php-fpm.pid 前面的分号

 cd php-fpm.d
 
 cp www.conf.default www.conf
 
 vim www.conf
② 修改user和group的用户为当前用户(也可以不改，默认会添加nobody这个用户和用户组)

【启动PHP】

  /etc/init.d/php-fpm start        #php-fpm启动命令
 
  /etc/init.d/php-fpm stop         #php-fpm停止命令
 
  /etc/init.d/php-fpm restart        #php-fpm重启命令
 
  ps -ef | grep php 或者 ps -A | grep -i php  #查看是否已经成功启动PHP
新建index.php，输出phpinfo；显示成功，说明php安装成功。
