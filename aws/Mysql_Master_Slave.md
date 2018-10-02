# AWS EC2安装Mysql
## 安装mysql
```
sudo yum install mysql
sudo yum install mysql-server
sudo yum install mysql-devel
```
## 启动mysql
```
sudo service mysqld start
sudo service mysqld enable
```
## 设置root用户密码
```
/usr/bin/mysqladmin -u root password '12345abc'
```

## 编辑主数据库配置 /etc/my.cnf
```
server-id=1
innodb_flush_log_at_trx_commit=2
sync_binlog=1
log-bin=mysql-bin-1
```
## 在主库创建从库用户
```
CREATE USER 'magento_test'@'54.180.86.252' IDENTIFIED BY 'Magento_12345';
```
## 在主库授权给从库用户
```
grant replication slave on *.* to 'magento_test'@'54.180.86.252' identified by 'Magento_12345';
flush  privileges;
```
## 重启主库
```
sudo service mysqld restart
```
## 查看状态
```
show master status;
```

## 编辑从数据库配置 /etc/my.cnf
```
server-id=2
innodb_flush_log_at_trx_commit=2
sync_binlog=1
log-bin=mysql-bin-2
```
## 重启从库
```
sudo service mysqld restart
```
## 在从库设置主库信息
```
change master to master_host='54.180.91.227', master_user='magento_test',master_password='Magento_12345', master_log_file='mysql-bin-1.000001',master_log_pos=107;
```
## 开启从库
```
start slave;
```
## 停止从库
```
stop slave;
```
## 查看从库状态
```
show slave status;
```