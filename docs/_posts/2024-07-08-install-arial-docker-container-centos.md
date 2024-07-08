---
layout: post
title: "Cài font Arial lên docker container ở Server CentOS 7"
date:   2024-07-08
categories: [CentOS 7, CentOS, Docker, Container, Arial, Fonts]
---

## Tham khảo

- [HD cài font arial cho CentOS 7](https://superuser.com/questions/1153990/anyone-know-how-to-install-arial-fonts-on-centos-7){:target="_blank"}.

## Nguyên nhân sâu xa

Dockerize project c# .net 8 lên server CentOS 7. Project chạy API và có sử dụng lib cần tới font Arial.

Image base trong Dockerfile là **FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base**.

## Hướng dẫn

Đường dẫn thư mục fonts trên CentOS và trong Docker container đều là `/usr/share/fonts`. Cài trên CentOS 7 rồi map volume lại là oke.

![]({{ site.baseurl }}/assets/images/install-arial-docker-container-centos/arial_0.png)

### Chi tiết

Cài bằng lệnh:

`wget http://www.itzgeek.com/msttcore-fonts-2.0-3.noarch.rpm`

`rpm -Uvh msttcore-fonts-2.0-3.noarch.rpm`

Cài xong thì check `ls /usr/share/fonts` xem có chưa. Như ảnh là oke rồi

![]({{ site.baseurl }}/assets/images/install-arial-docker-container-centos/arial_1.png)

Sau đó sửa file **docker-compose** để map volume như dưới rồi run lại docker là ăn tiền.

```
volumes:
    - /usr/share/fonts:/usr/share/fonts
```


