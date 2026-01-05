---
title: docker command cp
date: 2022-03-20 03:01:03
tags:
  - Unix
  - Docker
  - Command
categories:
  - Docker
cover: /images/cover.jpg  # 站内图片
---
# copy file from container to computer / docker向宿主机传输文件

docker cp container_id:<docker容器内的路径> <本地保存文件的路径>
docker cp 10704c9eb7bb:/root/test.text /home/vagrant/test.txt
# copy file from computer to container / 宿主机向docker传输文件

docker cp 本地文件的路径 container_id:<docker容器内的路径>
docker cp  /home/vagrant/test.txt 10704c9eb7bb:/root/test.text
