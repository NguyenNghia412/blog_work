---
layout: post
title:  "Cấu hình jenkins + gitlab centos7"
date:   2022-07-20
categories: Khong biet
---
## Trên server, cài git
yum install git

## Cài plugin gitlab
Manage Jenkins > Manage Plugins > Available > Gõ Gitlab > Install

## Gán Gitlab api token cho Jenkins

### Gitlab
Đăng nhập vào gitlab

Avatar > Edit profile > Access Token
#### Tại đây, tạo access token
Đặt Token name, chọn scope (Tích hết), ấn Create personal access token

### Jenkins
Manage Jenkins > Manage Credentials > System > Global credentials (unrestricted)

Chọn Gitlab api access token, rồi copy access token của gitlab vào đây

## Cấu hình Gitlab connections
Manage Jenkins > Configure System > Gitlab connections

Connection name = gitlab-epic-staging
Gitlab host URL = https://gitlab.com
Credentials = Gitlab api token vừa tạo

Sau đó, ấn Test Connetion => Success là okie

## Tạo Credentials SSH user with private key

### Tạo SSH RSA key pair (Tạo ở đâu cũng được: Windows or Linux đều được)
ssh-keygen -b 2048 -t rsa 

### Gitlab
Avatar > Edit Profile > SSH Keys
Copy key trong file public cho vào đây

### Jenkins
Manage Jenkins > Manage Credentials > System > Global credentials (unrestricted)

Chọn SSH Username with private key > Copy key trong file private cho vào đây

## Tạo project trên jenkins
New Item > Nhập tên > Chọn Freestyle project

### Tại Source Code Management, chọn Git
Repository URL = Link ssh
Credentials = Credentials tạo bên trên
Branch Specifier (blank for 'any') = */staging (Nhập tên nhánh muốn build)

### Tại Build Triggers
Chọn Build when a change is pushed to GitLab

### Tại Build (Nếu muốn làm gì đó sau khi pull code từ nhánh)
Chọn Execute shell