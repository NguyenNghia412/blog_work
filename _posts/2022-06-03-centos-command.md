---
layout: post
title:  "Một vài lệnh linux trên centos7"
date:   2022-06-03
categories: Khong biet
---
## Tim thu muc khi biet ten file
find / -name <ten-file>
find / -name deploy.sh
## Remove all <none> images
docker images  | grep '<none>' | awk '{print $3}' | xargs  docker image rm -f {}
## Nginx port denied
If you use a port bigger than 1024 with root privilege, but still got this problem, that's may be caused by SELinux:

Check this port, say 8024, in segange port
### Show
sudo semanage port -l | grep http_port_t

If 8024 doesn't exist in the port list, add it into segange port
### Add
sudo semanage port -a -t http_port_t  -p tcp 8024

## Extract
tar -xzvf node_exporter-1.3.1.linux-amd64.tar.gz

## Chọn fish làm default shell
chsh -s $(which fish)

## list file by date
ls -lt

## update package
### Centos 7
yum -y update 

## install wget
### Centos 7
yum install wget

## install net-tools
### Centos 7
yum install net-tools

yum install psmisc