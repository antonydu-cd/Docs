# Ubuntu安装Magento
## 更新软件
```shell
sudo apt-get update
sudo apt-get install -y debconf-utils curl
```
### 安装Nginx服务器
```shell
sudo apt-get -y install nginx
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

### 安装mysql
sudo apt-get -y install mysql-server-5.7 mysql-client

sudo mysql_secure_installation

sudo mysql

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '12345abcD@';
FLUSH PRIVILEGES;

### SWAP FILE
free -h
sudo dd if=/dev/zero of=/swapfile count=4096 bs=1MiB
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

location / {
    # First attempt to serve request as file, then
    # as directory, then fall back to displaying a 404.
    try_files $uri $uri/ /index.php?$args;
    index   index.php index.html index.htm;
}
location ~ \.php$ {
    include snippets/fastcgi-php.conf;

    # With php7.0-cgi alone:
    #fastcgi_pass 127.0.0.1:9000;
    # With php7.0-fpm:
    fastcgi_pass unix:/run/php/php7.0-fpm.sock;
}