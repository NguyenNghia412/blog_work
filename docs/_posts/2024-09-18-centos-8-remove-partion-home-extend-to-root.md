---
layout: post
title: "Centos 8: Gộp dung lượng ở /home vào /root"
date:   2024-09-18
categories: [CentOS 8, Linux, Partition]
---
## Nguyên nhân

Có 1 con VM chạy trên VSphere, OS là CentOS 8. Có 2 partion /dev/mapper/cl_app-home là 141GB, /dev/mapper/cl_app-root là 70GB. /root bị full nên phải gộp /home vào cho nhiều.

## Tham khảo

- [Chi tiết đọc bài viết này](https://netshop-isp.com.cy/blog/how-to-extend-root-partition-using-home-partition-space-in-centos/){:target="_blank"}.


## Cách làm

### Backup và unmount /home

```
mkdir /temp && cp -a /home /temp/
umount -fl /home
```

### Xoá LVM /home

`lvremove /dev/mapper/cl_app-home`

### Extend /root

```
lvextend -L+120G /dev/mapper/cl_app-root
xfs_growfs /dev/mapper/cl_app-root
```

### Copy nội dung /home cũ về thư mục ban đầu

```
cp -a /temp/home /
rm -rf /temp
```

### Xoá dòng **/dev/mapper/cl_app-home** trong file `/etc/fstab`

Làm cái này để đảm bảo system không tìm vào partion `/home` khi boot.

`vi /etc/fstab`

### Đồng bộ hệ thống với các thay đổi
`dracut --regenerate-all --force`

=> Xong! Reboot lại xem oke ko.

`df -Ph`