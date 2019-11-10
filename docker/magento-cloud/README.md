# Install epel
sudo amazon-linux-extras install epel

# Install 软件源
sudo yum install https://centos7.iuscommunity.org/ius-release.rpm

# Install php7.2
sudo yum -y install php72u php72u-pdo php72u-mysqlnd php72u-opcache php72u-xml php72u-gd php72u-devel php72u-intl php72u-mbstring php72u-bcmath php72u-json php72u-iconv php72u-soap php72u-fpm

# Install composer
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/bin/composer

# Install git
sudo yum install -y git

# Install cloud cli
sudo curl -sS https://accounts.magento.cloud/cli/installer | php
source ~/.bashrc

# Install docker
sudo yum install -y docker
sudo systemctl enable docker
sudo systemctl start docker

# Install gem
sudo yum install -y gem

# Install docker-sync
sudo gem install docker-sync

# Install and upgrade python
sudo yum -y install python-pip
sudo pip install --upgrade pip

# 解决安装docker-compose的错误
sudo pip install --upgrade cffi
sudo rm -fr /usr/lib/python2.7/site-packages/requests
sudo rm -fr /usr/lib/python2.7/site-packages/requests-2.6.0-py2.7.egg-info
sudo pip install requests

# 安装docker-compose
sudo pip install docker-compose
sudo yum install ruby-devel

# Clone code
git clone https://github.com/magento/magento-cloud.git

# 安装依赖
composer install

# 生成docker-compose.yml 配置文件
./vendor/bin/ece-tools docker:build --mode="developer"

# 运行docker-sync
sudo /usr/local/bin/docker-sync start

# 运行服务
sudo docker-compose up

# Deploy Magento
sudo docker-compose run deploy cloud-deploy && sudo docker-compose run deploy magento-command deploy:mode:set developer

# Configure and connect Varnish.
sudo docker-compose run deploy magento-command config:set system/full_page_cache/caching_application 2 --lock-env

# clear cache
sudo docker-compose run deploy magento-command cache:clean
