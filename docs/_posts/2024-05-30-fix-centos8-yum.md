---
layout: post
title:  "Fix lỗi yum trên centos 8"
date:   2024-05-30
categories: [Linux, Centos, Yum]
---
### Bệnh
Failed to download metadata for repo ‘AppStream’ [CentOS] 

### Cách sửa

```
cd /etc/yum.repos.d/

sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*

sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

yum update -y
```


