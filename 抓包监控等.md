# docs
杂项（服务器、数据库等相关安装配置文档）
监控 抓包
-- iftop tcpdump wireshark  phptrace

yum install wireshark
tshark -n -t a -R http.request -T fields -e "frame.time" -e "ip.src" -e "http.host" -e "http.request.method" -e "http.request.uri"


phptrace
$ wget https://pecl.php.net/get/trace-1.0.0.tgz # 下载源码
$ tar -xf trace-1.0.0.tgz # 解压文件
$ cd trace-1.0.0/extension # 进入扩展目录

$ whereis php-config # 找到 php-config 的路径
$ phpize
$ ./configure --with-php-config=/usr/bin/php-config # 这里的 --with-php-config 是上一步找到的路径
$ make # 编辑
$ make test # 编译测试
$ make cli # 命令行工具
$ make install-all # 安装 php 扩展，命令行工具到 php 目录

php.ini 配置文件中增加以下配置信息。

 php -i | grep extension_dir
[phptrace]
extension=trace.so
phptrace.enabled=1

php -m | grep trace

--监控全部进程
/www/server/php/72/bin/phptrace  -p all  
