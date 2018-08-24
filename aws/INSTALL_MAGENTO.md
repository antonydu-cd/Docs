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
### 使用 systemctl 命令可将 Apache Web 服务器配置为在每次系统启动时启动。
```shell
sudo systemctl enable httpd
```
### 通过运行以下命令验证 httpd 是否已启用
```shell
sudo systemctl is-enabled httpd
```    
Apache httpd 提供的文件保存在称为 Apache 文档根目录的目录中。Amazon Linux Apache 文档根为 /var/www/html，默认情况下归根用户所有。