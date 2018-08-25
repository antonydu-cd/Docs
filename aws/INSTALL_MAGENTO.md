# AWS EC2安装Magento
## 更新软件
```shell
sudo yum update -y
```
### 安装Apache服务器
```shell
sudo yum install -y httpd
```
### 查看安装信息
```shell
yum info httpd
```
### 启动 Apache Web 服务器
```shell
sudo systemctl start httpd
```
### 重新启动Apache服务器
```shell
sudo systemctl restart httpd
```
### 使用 systemctl 命令可将 Apache Web 服务器配置为在每次系统启动时启动
```shell
sudo systemctl enable httpd
```
### 通过运行以下命令验证 httpd 是否已启用
```shell
sudo systemctl is-enabled httpd
```    
Apache httpd 提供的文件保存在称为 Apache 文档根目录的目录中。Amazon Linux Apache 文档根为 /var/www/html，默认情况下归根用户所有。
### 设置文件权限
#### 将用户 (这里指 ec2-user) 添加到 apache
```shell
sudo usermod -a -G apache ec2-user
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
sudo chown -R ec2-user:apache /var/www
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
grep 'temporary' /var/log/mysqld.log
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

### 安装php7.1
```shell
sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum install -y http://dl.iuscommunity.org/pub/ius/stable/CentOS/7/x86_64/ius-release-1.0-14.ius.centos7.noarch.rpm
sudo yum -y update
sudo yum -y install php71u php71u-pdo php71u-mysqlnd php71u-opcache php71u-xml php71u-mcrypt php71u-gd php71u-devel php71u-intl php71u-mbstring php71u-bcmath php71u-json php71u-iconv php71u-soap
```
### 修改内存限制
```shell
sudo sed -i 's/memory_limit = 128M/memory_limit = -1/g' /etc/php.ini
```

# Install Composer
```shell
curl -sS https://getcomposer.org/installer | php
```
```shell
sudo mv composer.phar /usr/bin/composer
```

### 设置登录密码
```shell
sudo passwd ec2-user
```
### 更新 /etc/ssh/sshd_config 文件中的 PasswordAuthentication 参数：
```shell
PasswordAuthentication yes
```
### 重新启动 SSH 服务
```shell
sudo service sshd restart
```

### composer install magento2
#### 切换到网站根目录
```shell
cd /var/www/html
```
#### 安装Magento2
```shell
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition .
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
        --base-url "http://34.220.183.209/" \
        --backend-frontname "SiteAdmin" \
        --db-host "127.0.0.1" \
        --db-name "magento_test" \
        --db-user "magento_test" \
        --db-password "Magento_12345" \
        --session-save "files" \
        --use-rewrites "1"
```
### 开启apache url rewrite
