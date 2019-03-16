### 安装Nginx服务器
```shell
sudo yum install -y nginx
```
### 查看安装信息
```shell
yum info nginx
```
### 使用 service 命令可将 nginx Web 服务器配置为在每次系统启动时启动
```shell
sudo service nginx enable
Usage: /etc/init.d/nginx {start|stop|reload|configtest|status|force-reload|upgrade|restart|reopen_logs}
```
### 启动 nginx Web 服务器
```shell
sudo service nginx start
```
### 重新启动nginx服务器
```shell
sudo service nginx reload
```

### 安装mysql客户端
```shell
sudo yum -y install mysql
```
### 安装mysql服务器
```shell
sudo yum -y install mysql-server
```
### 使用 service 命令可将 mysqld配置为在每次系统启动时启动
```shell
sudo service mysqld enable
Usage: /etc/init.d/mysqld {start|stop|status|restart|condrestart|try-restart|reload|force-reload}
```
### 启动mysqld服务
```shell
sudo service mysqld start
```
### 重新启动nginx服务器
```shell
sudo service mysqld reload
```

### 安装php7.2
sudo yum -y install php72 php72-pdo php72-mysqlnd php72-opcache php72-xml php72-mcrypt php72-gd php72-devel php72-intl php72-mbstring php72-bcmath php72-json php72-iconv php72-soap php72-fpm
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