---
layout: post
title:  "Cài nfs trên centos7"
date:   2022-08-17
categories: [Linux, CentOS, nfs]
---

## Setup nfs server
### Installing nfs-utils
yum install nfs-utils

### Choose the directory to share. If not present create one.
mkdir /var/nfs_share_dir

### Add permissions and ownwership privilages to the shared directory.
chmod -R 755 /var/nfs_share_dir
chown nfsnobody:nfsnobody /var/nfs_share_dir

### Start the nfs services.
systemctl enable rpcbind
systemctl enable nfs-server
systemctl enable nfs-lock
systemctl enable nfs-idmap
systemctl start rpcbind
systemctl start nfs-server
systemctl start nfs-lock
systemctl start nfs-idmap

### Configuring the exports file for sharing.
vi /etc/exports
/var/nfs_share_dir    192.168.48.101(rw,sync,no_root_squash)
/var/nfs_share_dir    *(rw,sync,no_root_squash)

### Restart the service
systemctl restart nfs-server

### Only for Centos 7,NFS service override
firewall-cmd --permanent --zone=public --add-service=nfs
firewall-cmd --permanent --zone=public --add-service=mountd
firewall-cmd --permanent --zone=public --add-service=rpc-bind
firewall-cmd --reload

## Setup nfs client

--====================================================
Nếu lỗi ở client ko mount được, thì fix như sau ở nfs server
# cat /etc/exports 
/home/export *(rw,sync,no_root_squash,insecure)
# exportfs -rav