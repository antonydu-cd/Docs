# Centos安装 Vue Storefront
## 安装Docker
```
sudo yum update
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce
sudo systemctl start docker
sudo systemctl enable docker
```
## 安装nodejs
```
wget https://nodejs.org/dist/v8.12.0/node-v8.12.0-linux-x64.tar.xz
tar -xvf node-v8.12.0-linux-x64.tar.xz
mv node-v8.12.0-linux-x64 node
mv node /usr/local/
ln -s /usr/local/node/bin/node /usr/bin/node
ln -s /usr/local/node/bin/npm /usr/bin/npm
```
## 安装Yarn
 ```
 sudo npm install yarn -g
 ln -s /usr/local/node/bin/yarn /usr/bin/yarn
 ```
## 安装Vue Storefront
```
mkdir /var/www/project
cd /var/www/project
git clone https://github.com/DivanteLtd/vue-storefront.git
cd vue-storefront
yarn install
yarn installer
```