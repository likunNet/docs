# docs
杂项（服务器、数据库等相关安装配置文档）


linux下的nginx日志自动备份压缩--日志切割机
部署完毕nginx之后，发现自己的/var/log/nginx/*log的日志不会压缩，一直都是一个文本写日志，

时间久了，日志文件内存过于增加，将会导致在日志添加过程效率降低，延长时间。

默认安装的nginx都会每天凌晨自动去备份，但是也有nginx 不会自动备份压缩日志，

于是我们先使用命令看看配置： 

   cat /etc/logrotate.d/nginx 
 

当然也可以直接修改   

vim  /etc/logrotate.d/nginx 
 

然后把我下面的命令粘贴进去

复制代码
/var/log/nginx/*log {
create 0644 nobody root
daily
rotate 40
missingok
notifempty
compress
sharedscripts
postrotate
/bin/kill -USR1 `cat /run/nginx.pid 2>/dev/null` 2>/dev/null || true
endscript
}
复制代码
保存一下，第二天就可以看看你的日志是否自动切割压缩了。

我们也可以输入命令，进行测试，看看我们的配置代码是否能运行

logrotate -d /etc/logrotate.d/nginx
回车之后，就会展示读取nginx的相关配置，如果出现报错的英文，说明你的日志路径 不是默认的  /var/log/nginx/*log

此时你就修改一下 你指定的日志默认路径。

注：下面的代码 nobody 是我的nginx.confi里面的用户组，你可以看看你的配置用户组叫什么名字，默认是nobody，当然也www-data的用户组，自己对比观察一下。

create 0644 nobody root

ip 统计
awk '{print $1}'  /var/log/nginx/pet_api_bk0325.log |sort | uniq -c | sort -n -r |head -n 20
 -- 接口访问统计
cat /var/log/nginx/pet_api.log |awk '{print$7}'|sort|uniq -c |sort -rn

 cat /var/log/nginx/pet_api.log |awk '{print$7}'|sort|uniq -c |sort -rn |head -10
 
 cat  /var/log/nginx/pet_api_error.log | awk '{a[$17]++} END {for(b in a) print b"\t"a[b]}' | sort -k2 -r | head -n 10
 
 nginx访问量统计
 
0.查询某个时间段的日志
 
cat appapi.dayutang.cn.access.log |grep 'POST'|grep '2019:10' > 20191059.log
 
1.根据访问IP统计UV
 
awk '{print $1}' 20190959.log|sort | uniq -c |wc -l
 
2.统计访问URL统计PV
 
awk '{print $8}' 20190959.log|wc -l
 
3.查询访问最频繁的URL
 
awk '{print $8}' 20190959.log|sort | uniq -c |sort -n -k 1 -r|more
 
4.查询访问最频繁的IP
 
awk '{print $1}' 20190959.log|sort | uniq -c |sort -n -k 1 -r|more
 
5.根据时间段统计查看日志
 
cat 20190959.log| sed -n '/14\/Mar\/2015:21/,/14\/Mar\/2015:22/p'|more
 
6.查询每秒请求
awk '{print $4}' 20190959.log |cut -c 14-21|sort|uniq -c|sort -nr|head -n 100
