# docs
杂项（服务器、数据库等相关安装配置文档）
转载至以下博客
https://www.pianshen.com/article/5025305418/
https://blog.csdn.net/ffzhihua/article/details/88846642

环境
系统：CentOS 7.5
软件：php-fpm-exporter.linux.amd64

准备
PHP-FPM端
相关连接 https://easyengine.io/tutorials/php/fpm-status-page

https://blog.csdn.net/ffzhihua/article/details/88844259

配置PHP-FPM

# vim /etc/php-fpm.d/www.conf
pm.status_path = /status
ping.path = /ping
重启PHP-FPM

# systemctl restart php-fpm
Nginx端
添加Nginx配置

# vim /etc/nginx/conf.d/php-fpm-status.conf
server {
        listen 9010; 
        allow 127.0.0.1;
        deny all;
 
        location ~ ^/(status|ping)$ {
                fastcgi_pass 127.0.0.1:9000;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
        }
}
重启Nginx

# systemctl restart nginx
 

PHP-FPM-exporter端
下载php-fpm-exporter
地址：https://github.com/bakins/php-fpm-exporter/releases

安装php-fpm-exporter

# mkdir -p /usr/local/prometheus/php-fpm-exporter
 
# mv php-fpm-exporter.linux.amd64 /usr/local/prometheus/php-fpm-exporter/php-fpm-exporter
 
# chmod +x /usr/local/prometheus/php-fpm-exporter/php-fpm-exporter
 

启动php-fpm-exporter

# nohup /opt/php-fpm-exporter/php-fpm-exporter --addr 0.0.0.0:9190 --endpoint http://127.0.0.1/status &
 
Prometheus端
配置Prometheus

# vim /usr/local/prometheus/prometheus.yml
 
scrape_configs:
 
- job_name: 'PHP-FPM'
 
static_configs:
 
- targets:
 
- php-local.com:9190
 

 

重启Prometheus

# systemctl restart prometheus
Grafana端
模板地址 https://grafana.com/dashboards/3901

添加dashboards
点击Create - Import，输入dashboards的id（推荐）
