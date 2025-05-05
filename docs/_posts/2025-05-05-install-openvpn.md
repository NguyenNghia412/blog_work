---
layout: post
title: "Cài đặt OpenVPN cho Ubuntu Server 22 04 (Năm 2025)"
date:   2025-05-05
categories: [Linux, Ubuntu, OpenVPN, vpn]
---

## Các loại link

- [Hướng dẫn cài OpenVPN từ trang chủ Ubuntu](https://documentation.ubuntu.com/server/how-to/security/install-openvpn/index.html){:target="_blank"}.
- [Hướng dẫn tạo file opvn cho client để kết nối đến VPN Server](https://serverfault.com/questions/483941/generate-an-openvpn-profile-for-client-user-to-import){:target="_blank"}.
- [Link tải Open VPN Connect cho Windows](https://openvpn.net/client/){:target="_blank"}.

## Install the server (Cài thư viện cần thiết)

`sudo apt install openvpn easy-rsa`

## Set up the Public Key Infrastructure (PKI)

### Set up the Certificate Authority (Tạo CA. Server CA có thể chung với VPN Server hoặc nằm riêng ở CA Server)

```
sudo make-cadir /etc/openvpn/easy-rsa
cd /etc/openvpn/easy-rsa
./easyrsa init-pki
./easyrsa build-ca
```

CA creation complete and you may now import and sign cert requests.
Your new CA certificate file for publishing is at: `/etc/openvpn/easy-rsa/pki/ca.crt`

### Create server keys and certificates (Tạo key và cert cho vpn server)

Next, we will generate a key pair for the server:

```
./easyrsa gen-req [myservername] nopass
./easyrsa gen-dh
./easyrsa sign-req server [myservername]
cp pki/dh.pem pki/ca.crt pki/issued/[myservername].crt pki/private/[myservername].key /etc/openvpn/
```

### Create client certificates (Tạo key và cert cho vpn client)

```
./easyrsa gen-req myclient1 nopass
./easyrsa sign-req client myclient1
```

## Simple server configuration (Cấu hình cho vpn server)

```
ls -l /usr/share/doc/openvpn/examples/sample-config-files/
cp /usr/share/doc/openvpn/examples/sample-config-files/server.conf /etc/openvpn/server.conf
vi /etc/openvpn/server.conf
```

Sửa nội dung file **server.conf** như dưới đây

'''

ca ca.crt

cert [myservername].crt

key [myservername].key

dh dh.pem

'''

Complete this set with a TLS Authentication (TA) key in etc/openvpn for tls-auth like this:

`sudo openvpn --genkey --secret ta.key`

`vi /etc/sysctl.conf`

Sửa nội dung file **sysctl.conf** như dưới đây

'''

#net.ipv4.ip_forward=1

'''

```
sudo sysctl -p /etc/sysctl.conf
sudo systemctl start openvpn@[myserver]
```

Khởi động service openvpn sẽ đi theo từng template định dạng conf bên trong **/etc/openvpn**, như ví dụ bên trên, file conf có tên **/etc/openvpn/server.conf** nên sẽ là **openvpn@server**.

Be aware that the systemctl start openvpn is not starting the openvpn you just defined. OpenVPN uses templated systemd jobs, openvpn@CONFIGFILENAME. So if, for example, your configuration file is myserver.conf your service is called openvpn@myserver. You can run all kinds of service and systemctl commands like start/stop/enable/disable/preset against a templated service like openvpn@server.

Check dịch vụ chạy

```
sudo systemctl start openvpn@server
sudo journalctl -u openvpn@server -xe
```

Check xem đã tạo tunnel chưa. Dải IP mặc định sẽ là *10.8.0.0/24*

`ip addr show dev tun0`

## Một số location của các file

| File | Đường dẫn |
| myclient1.req | /etc/openvpn/easy-rsa/pki/reqs |
| myclient1.crt | /etc/openvpn/easy-rsa/pki/issued |
| myclient1.key | /etc/openvpn/easy-rsa/pki/private |
| ca.crt | /etc/openvpn |
| ta.crt | /etc/openvpn |

## VPN Profile cho Client

```
client
proto udp
remote [IP hoặc tên miền]
port 1194
dev tun
nobind

key-direction 1

<ca>
-----BEGIN CERTIFICATE-----
# insert base64 blob from ca.crt
-----END CERTIFICATE-----
</ca>

<cert>
-----BEGIN CERTIFICATE-----
# insert base64 blob from client1.crt
-----END CERTIFICATE-----
</cert>

<key>
-----BEGIN PRIVATE KEY-----
# insert base64 blob from client1.key
-----END PRIVATE KEY-----
</key>

<tls-auth>
-----BEGIN OpenVPN Static key V1-----
# insert ta.key
-----END OpenVPN Static key V1-----
</tls-auth>
```
