# docs
# homebrew https://brew.sh/
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

# homebrew 加速源: https://mirror.tuna.tsinghua.edu.cn/help/homebrew/
git -C "$(brew --repo)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
brew tap homebrew/core
git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
brew tap homebrew/cask
git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git
brew update
# 复原
# git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew.git
# git -C "$(brew --repo homebrew/core)" remote set-url origin https://github.com/Homebrew/homebrew-core.git
# git -C "$(brew --repo homebrew/cask)" remote set-url origin https://github.com/Homebrew/homebrew-cask.git
# brew update

# homebrew-bottles
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile

# iterm2
brew cask install iterm2

# vim: 可以命令行使用 code 命令, 使用 vscode 编辑文件
# brew install vim

# fish shell: 推荐使用
brew install fish
# sudo echo '/usr/local/bin/fish' >> /etc/shells
# chsh -> 切换默认shell
# fish_config -> 配置 fish
# fish 配置目录: ~/.config/fish

# php: 项目中目前统一使用 7.2
# 踩到的坑: 本地 7.3 更新 composer 后, 有的包更新后强制要求使用 7.3, 导致其他环境 composer i 失败
brew install php@7.2
# 安装后注意根据提示添加 PATH
# composer: https://developer.aliyun.com/composer
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
# pecl: http://pecl.php.net/
# 可以在 pecl 上下载好包后直接安装, 避免重复网络请求
# swoole
# 确保环境变量里能找到 openssl 的编译库
pecl install swoole-4.4.13.tgz
# 使用 `php --ini` 查找 php.ini 文件位置, 编辑添加 `swoole.use_shortname = 0`, 使用 `php --ri swoole` 确认是否生效
pecl install redis-5.1.1.tgz
pecl install mongodb-1.6.1.tgz
# protobuf
brew install protobuf
pecl install protobuf-3.11.1.tgz
# kafka
brew install librdkafka
pecl install rdkafka-4.0.0.tgz
# zookeeper
brew install zookeeper
pecl install zookeeper-0.6.4.tgz


# nginx
brew install nginx

修改权限
sudo chown root:wheel /usr/local/Cellar/nginx/1.15.2/bin/nginx

sudo chmod u+s /usr/local/Cellar/nginx/1.15.2/bin/nginx

sudo chown -R root:wheel /usr/local/etc/nginx/

修改php-fpm配置
sudo cp /private/etc/php-fpm.conf.default /private/etc/php-fpm.conf
vim /private/etc/php-fpm.conf

error_log = /usr/local/var/log/php-fpm.log
修改配置文件端口号及 php配置
 /usr/local/etc/nginx/nginx.conf
 
 
 
 location ~ \.php$ {

  root      html;

  fastcgi_pass  127.0.0.1:9000;

  fastcgi_index index.php;

  fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

  include    fastcgi_params;

}

nginx -t

nginx -s reload

brew services restart nginx


sudo php-fpm -D

> sudo killall php-fpm
~> sudo lsof -i:9000


mysql

brew install mysql@5.7
brew services start mysql@5.7
sudo ln -s /usr/local/opt/mysql@5.7/bin/mysql /usr/bin
 alias mysql=/usr/local/opt/mysql@5.7/bin/mysql
 修改密码
 update mysql.user set authentication_string = password('123456') where user='root';
 flush privileges;
 重启生效

