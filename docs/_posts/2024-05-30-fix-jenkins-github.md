---
layout: post
title:  "Fix lỗi jenkins với github trên centos7"
date:   2024-05-30
categories: [Linux, Centos, Jenkins, Github]
---
### Bệnh
- Đã tạo ssh key, đã copy public key lên account github và sử dụng private key tạo ssh credentials trên jenkins. Nhưng khi tạo nhập thông tin repo vào items ở jenkins thì báo lỗi như ảnh dưới:

![]({{ site.baseurl }}/assets/images/fix-jenkins-github-account-ssh.png)

### Cách sửa

Đăng nhập vào bằng jenkins account:

`su - jenkins -s /bin/bash`

Chạy lệnh sau:

`ssh -T git@github.com`

Restart lại jenkins service:

`systemctl restart jenkins`