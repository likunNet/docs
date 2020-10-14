# docs
杂项（服务器、数据库等相关安装配置文档）
1、安装git

yum install -y git

2、创建git用户

useradd git -g git


2、创建证书登录
收集所有需要登录的用户的公钥，公钥位于id_rsa.pub文件中，把我们的公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

如果没有该文件创建它：

$ cd /home/git/
$ mkdir .ssh
$ chmod 755 .ssh
$ touch .ssh/authorized_keys
$ chmod 644 .ssh/authorized_keys


3、创建git仓库

设置 /home/gitrepo为 Git 各仓库总目录

然后把 Git 仓库的 owner 修改为 git

 mkdir -p /home/gitrepo
 
 chown git:git /home/gitrepo/
 
 cd /home/gitrepo
 
 git init --bare /home/gitrepo/gittest.git
 
 chown -R git:git gittest.git/
 
 
 
 4、客户端配置ssh-key 并clone仓库
 
 ssh-keygen -t rsa -C "youremail@example.com"  
 
 5、Git服务器打开RSA认证 
 将我们的公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个
 然后就可以去Git服务器上添加你的公钥用来验证你的信息了。
 在Git服务器上首先需要将/etc/ssh/sshd_config中将RSA认证打开，即：

1.RSAAuthentication yes     
2.PubkeyAuthentication yes     
3.AuthorizedKeysFile  .ssh/authorized_keys
 
 
 git clone git@ip:/home/gitrepo/gittest.git
 
 6、服务器配置钩子自动同步代码
 
 cd /home/gitrepo/gittest.git/hooks
vi post-receive
#!/bin/bash

cd /home/git/gittest.git
git --work-tree=/web/www/gittest checkout -f


保存文件并修改权限  work-tree配置任意目录 
chown git:git post-receive
chmod 744 post-receive
 
 配置目录权限
 usermod -aG root git 
chmod -R 775 /web/www/gittes
 
 7、客户端：连接git服务器
git init
git remote add origin git@ip:/data/git/gittest.git
git add.
git commit 
git push -u origin master -f
 
 备注：服务端可以禁用git登陆
 禁用git用户的shell登陆
出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

git:x:1001:1001:,,,:/home/git:/bin/bash  
最后一个冒号后改为：

git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell  
这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

参考：
https://www.cnblogs.com/chinaifae/articles/10174382.html
https://www.cnblogs.com/jimmy-muyuan/p/8762934.html
