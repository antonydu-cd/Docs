# ALI ECS安装Magento
## 更新软件
```shell
sudo yum update -y
```
### 启用EPEL rpm软件包
```shell
sudo amazon-linux-extras install epel
```
或者
```shell
sudo yum install -y epel-release
```
### 安装Nginx服务器
```shell
sudo yum install -y nginx
```
### 查看安装信息
```shell
yum info nginx
```
### 使用 systemctl 命令可将 nginx Web 服务器配置为在每次系统启动时启动
```shell
sudo systemctl enable nginx
```
### 启动 nginx Web 服务器
```shell
sudo systemctl start nginx
```
### 重新启动nginx服务器
```shell
sudo systemctl restart nginx
```
### 设置文件权限
#### 将用户 (这里指 ec2-user) 添加到 apache
```shell
sudo usermod -a -G nginx ec2-user
```
#### 先退出再重新登录以选取新组，然后验证您的成员资格
##### 退出 (使用 exit 命令或关闭终端窗口)：
```shell
exit
```
##### 要验证您是否为 apache 组的成员，请重新连接到实例，然后运行以下命令：
```shell
groups
```
#### 将 /var/www 及其内容的组所有权更改到 apache 组
```shell
sudo chown -R ec2-user:nginx /var/www
```
#### 要添加组写入权限以及设置未来子目录上的组 ID，请更改 /var/www 及其子目录的目录权限
```shell
sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;
```
#### 要添加组写入权限，请递归地更改 /var/www 及其子目录的文件权限
```shell
find /var/www -type f -exec sudo chmod 0664 {} \;
```

### 安装mysql源
```shell
sudo yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
```
### 安装mysql
```shell
sudo yum -y install mysql-community-server
```
### 启动mysql服务器
```shell
sudo systemctl start mysqld
```
### 如果您希望每次启动时 MariaDB 服务器都启动，请键入以下命令。
```shell
sudo systemctl enable mysqld
```
### 通过一下命令验证mysql是否已启用
```shell
sudo systemctl is-enabled mysqld
```
### 查看临时密码
```shell
sudo grep 'temporary' /var/log/mysqld.log
```
### 修改密码
```shell
sudo mysql_secure_installation
```
### MySQL 数据库创建用户和密码
```shell
CREATE USER 'magento_test'@'localhost' IDENTIFIED BY 'Magento_12345';
```
### 创建数据库
```shell
CREATE DATABASE `magento_test`;
```
###  给用户magento_test授予数据库magento_test的完全访问权限
```shell
GRANT ALL PRIVILEGES ON `magento_test`.* TO "magento_test"@"localhost";
```
### 刷新数据库权限以接受您的所有更改
```shell
FLUSH PRIVILEGES;
```
### 只安装client：
```shell
rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
yum -y install mysql-community-client
```

### 安装php7.2
```shell
sudo yum install https://centos7.iuscommunity.org/ius-release.rpm
sudo yum -y install php72u php72u-pdo php72u-mysqlnd php72u-opcache php72u-xml php72u-gd php72u-devel php72u-intl php72u-mbstring php72u-bcmath php72u-json php72u-iconv php72u-soap php72u-fpm
```

```shell
sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum install -y http://dl.iuscommunity.org/pub/ius/stable/CentOS/7/x86_64/ius-release-1.0-15.ius.centos7.noarch.rpm
sudo yum -y update
sudo yum -y install php71u php71u-pdo php71u-mysqlnd php71u-opcache php71u-xml php71u-mcrypt php71u-gd php71u-devel php71u-intl php71u-mbstring php71u-bcmath php71u-json php71u-iconv php71u-soap php71u-fpm
```
```shell
sudo yum -y install php70u php70u-pdo php70u-mysqlnd php70u-opcache php70u-xml php70u-mcrypt php70u-gd php70u-devel php70u-intl php70u-mbstring php70u-bcmath php70u-json php70u-iconv php70u-soap php71u-fpm
```
### 修改内存限制
```shell
sudo sed -i 's/memory_limit = 128M/memory_limit = -1/g' /etc/php.ini
```

### 修改php-fpm配置 /etc/php-fpm.d/www.conf
```shell
listen = /run/php-fpm/www.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0664
user = nginx
group = nginx
```

### 配置nginx虚拟机 /etc/nginx/nginx.conf
```shell
server {
    listen 80;
    server_name _;
    root   /var/www/html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
}
```

### 配置php-fpm：新增 /etc/nginx/default.d/php.conf
```shell
index index.php index.html index.htm;

location ~ \.(php|phar)(/.*)?$ {
    fastcgi_split_path_info ^(.+\.(?:php|phar))(/.*)$;

    fastcgi_intercept_errors on;
    fastcgi_index  index.php;
    include        fastcgi_params;
    fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    fastcgi_param  PATH_INFO $fastcgi_path_info;
    fastcgi_pass   127.0.0.1:9000;
}
```

```
### 启动php-fpm服务
```shell
sudo systemctl start php-fpm
```

# Install Composer
```shell
curl -sS https://getcomposer.org/installer | php
```
```shell
sudo mv composer.phar /usr/bin/composer
```
### SWAP FILE
free -h
sudo dd if=/dev/zero of=/swapfile count=4096 bs=1MiB
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

### composer install magento2
#### 切换到网站根目录
```shell
cd /var/www/html
```
#### 安装Magento2
```shell
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition .
```
```shell
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
```
```shell
composer require magento/extension-b2b
```
### 安装sample-data
```shell
php bin/magento sampledata:deploy
```
### 命令行安装
```shell
php -f bin/magento setup:install \
        --admin-firstname "antony" \
        --admin-lastname "du" \
        --admin-email "antony@ebrook.com.tw" \
        --admin-user "antony.du" \
        --admin-password "12345abc" \
        --base-url "http://emea.ebrook.xyz/" \
        --backend-frontname "SiteAdmin" \
        --db-host "database.c8gzv9exvckw.ap-northeast-1.rds.amazonaws.com" \
        --db-name "magento232" \
        --db-user "root" \
        --db-password "Magento_12345" \
        --session-save "files" \
        --use-rewrites "1"
```

    upstream fastcgi_backend {
        server  unix:/run/php-fpm/www.sock;
    }

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  magento2.8net.com;
	    return 301 https://$server_name$request_uri;
    }

    server {
        listen       443 ssl http2 default_server;
        listen       [::]:443 ssl http2 default_server;
        server_name  magento2.8net.com;
	index  index.php index.html index.htm;
        set $MAGE_ROOT /var/www/html;
        include /var/www/html/nginx.conf.sample;

	ssl on;
        ssl_certificate "/etc/pki/nginx/server.crt";
        ssl_certificate_key "/etc/pki/nginx/server.key";
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }