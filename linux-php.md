# docs


rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm

yum -y install php72w
yum -y install php72w-cli php72w-common php72w-devel php72w-mysql  php72w-fpm


rpm -Uvh http://repo.mysql.com/mysql57-community-release-el7-7.noarch.rpm



1、mysql安装 5.7
rpm -ivh mysql57-community-release-el7-10.noarch.rpm
 yum -y install mysql57-community-release-el7-10.noarch.rpm
 sudo yum module disable mysql
 
 yum -y install mysql-community-server
 
 或者
 rpm -Uvh http://repo.mysql.com/mysql57-community-release-el7-7.noarch.rpm
yum install mysql-server mysql-devel mysql


php-fpm
nginx

location ~ \.php$ {
         fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;

        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_param PATH_INFO $fastcgi_path_info;
        fastcgi_param  SCRIPT_FILENAME  /www/web$fastcgi_script_name;
        fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;

          include        fastcgi_params;
        }


2 php-swoole

yum install autoconf
pecl install swoole

php-fpm安装详解
https://blog.csdn.net/aerchi/article/details/83858180


scp -P22   -r -i ~/.ssh/dongjing-shanghai.pem root@kiri_pro01:/data/backup/back_from_japan_kiriwoodinc_bak ./

ssh时指定密钥：

ssh -p22 -i ~/.ssh/dongjing-shanghai.pem root@kiri_pro01
