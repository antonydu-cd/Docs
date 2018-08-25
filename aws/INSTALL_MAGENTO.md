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