1, 运行Jenkins
```shell
docker run -d -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
```
2, 运行Gitlab
```shell
sudo docker run --detach \
--publish 443:443 --publish 80:80 --publish 22:22 \
--name gitlab \
--restart always \
--volume ~/gitlab/config:/etc/gitlab \
--volume ~/gitlab/logs:/var/log/gitlab \
--volume ~/gitlab/data:/var/opt/gitlab \
gitlab/gitlab-ce:latest
```