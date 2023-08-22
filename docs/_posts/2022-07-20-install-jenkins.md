---
layout: post
title:  "Cài jenkins trên centos7"
date:   2022-07-20
categories: [Linux, Centos, Jenkins]
---
### Install jdk
`yum install java-1.8.0-openjdk-devel`

### Enable repo jenkins
`curl --silent --location http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo | sudo tee /etc/yum.repos.d/jenkins.repo`

### Add repo to sytem
`rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key => Ver cũ`

`sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key => 2023`

### Install latest stable version
`yum install jenkins -y`

`systemctl start jenkins`

`systemctl enable jenkins`

`systemctl status jenkins`

### Nếu có semanage thì chạy lệnh (Ko có thì thoai)
`sudo semanage port -a -t http_port_t  -p tcp 8080`

### Mở port của firewall (Jenkins chạy cổng mặc định là 8080)
`firewall-cmd --permanent --zone=public --add-port=8080/tcp`

`firewall-cmd --reload`

`firewall-cmd --permanent --zone=public --list-ports`

### Các bước cuối
1. Truy cập [đường dẫn](http://localhost:8008)

1. Lấy default administrator password

    `cat /var/lib/jenkins/secrets/initialAdminPassword`

2. Xong là ấn cài các kiểu trên WEB GUI

### Create First Admin User
admin/agalkjvlas