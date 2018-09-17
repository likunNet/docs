记录CentOS7.X版本下安装MySQL5.7数据库
 设置rpm下载目录在/opt目录下新建一个目录存放mysql
        
         cd/opt
         sudo mkdir mysql

下载MySQL的源
    
    wget http://repo.mysql.com/mysql57-community-release-el7-8.noarch.rpm

如果在这之前没有提示-bash: wget: command not found，那么还得先安装wget

        sudo yum install wget

安装MySQL的rpm

    sudo rpm -ivh mysql57-community-release-el7-8.noarch.rpm

安装MySQL

    sudo yum install mysql-server

在这步中只需要一直y然后回车就行，数据库就安装好了，现在进行连接：

    mysql -uroot -p
    root
    ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)

这是/var/lib/mysql权限问题，修改MySQL权限为当前用户

    sudo chown -R xxx:xxx /var/lib/mysql
    xxx为当前的用户名以及所属组 
重启MySQL服务
    
    service mysqld restart
    
密码输入root即可 
如果还不行的话通过这个命令获得初始密码

    cat /var/log/mysqld.log  | grep password

其中A temporary password is generated for root@localhost: bGlY?13TtFyX–：后面的就是初始密码；登陆进去之后先不做其他操作，作如下操作：

    set global validate_password_policy=0;
    set global validate_password_length=4;
设置密码

    alter user 'root'@'localhost' identified by 'root';
自此CentOS安装MySQL5.7就完美安装了

     mysql -uroot -p
    Enter password:
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 4
    Server version: 5.7.20 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.


