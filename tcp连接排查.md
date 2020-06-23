# docs

— 查看 tcp连接总数
netstat -na|wc -l


—  查看某个tcp连接状态 总数
 netstat -na|grep ESTABLISHED|wc -l

— 查看连接状态统计数据
netstat -na | awk '{print $6}' | sort -n | uniq -c

— 查看某个连接状态进程信息
netstat -nap | grep CONN 

— 查看tcp 连接状态进程统计
netstat -nap | grep CONN | grep -v '-' | awk '{print $8}' | awk -F'/' '{print $1}' | sort -n | uniq -c


netstat -nap | grep TIME_WAIT | grep -v '-' | awk '{print $8}' | awk -F'/' '{print $1}' | sort -n | uniq -c


date;netstat -nap | grep CONN | grep -v '-' | awk '{print $8}' | awk -F'/' '{print $1}' | sort -n | uniq -c | grep 2839

while true; do date;netstat -nap | grep CONN | grep -v '-' | awk '{print $8}' | awk -F'/' '{print $1}' | sort -n | uniq -c | grep 2839; sleep 1; done

— 监控进程信息
strace -p 2839
lsof -p 2838
crontab -l | grep video

lsof -p 27354 -nP |grep 'TCP'
