记录CentOS7.X版本mysql主从配置 
主库

    log-error=/var/log/mysqld.log
    pid-file=/var/run/mysqld/mysqld.pid
    log-bin = mysql-bin
    server-id =204
    innodb-file-per-table =ON
    skip_name_resolve=ON 
    binlog-ignore-db=information_schema
    binlog-ignore-db=mysql
这里的server-id用于标识唯一的数据库，在从库必须设置为不同的值。

binlog-ignore-db：表示同步的时候忽略的数据库

binlog-do-db：指定需要同步的数据库

1、修改完配置，重启mysql

systemctl restart mysql

从库
        
        log-bin=mysql-bin
        server-id=201
        binlog-ignore-db=information_schema
        binlog-ignore-db=cluster
        binlog-ignore-db=mysql
        replicate-ignore-db=mysql
        log-slave-updates
        slave-skip-errors=all
        slave-net-timeout=60 
        innodb_flush_log_at_trx_commit=2
        sync_binlog=1

主库创建slave 账号
    
    ## 创建 test 用户，指定该用户只能在主库 192.168.78.130 上使用 MyPass1! 密码登录
    mysql> create user 'test'@'192.168.78.130' identified by 'MyPass1!';
    
    ## 为 test 用户赋予 REPLICATION SLAVE 权限。
    mysql> grant replication slave on *.* to 'test'@'192.168.78.130';
    
    ## 查看用户
    mysql> select user,host from mysql.user;
    
    ## 查看 master 状态
    mysql> show master status;

    补充：
    grant FILE on *.* to 'root'@'192.168.0.4' identified by 'root';
    grant replication slave on *.* to 'root'@'192.168.0.4' identified by 'root';
    flush privileges;

从库：
    
    [sql] 
    mysql> stop slave;  
    Query OK, 0 rows affected (0.10 sec)  
    [sql] 
    -- 重新设置复制参数  
    mysql> change master to master_host='10.24.54.18',master_port=3306,master_user='replication',master_password='xxxxxx';  
    Query OK, 0 rows affected, 0 warnings (0.40 sec)  
       
    mysql> start slave;  
    Query OK, 0 rows affected (0.01 sec)  
       
    mysql>  show slave status\G;  
    
 记得检查 Slave_IO_Running、Slave_SQL_Running状态为yes 处理Last_Errno错误信息


  


