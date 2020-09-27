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



2 php-swoole

yum install autoconf
pecl install swoole

