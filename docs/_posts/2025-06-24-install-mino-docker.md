---
layout: post
title:  "Install minio (docker)"
date:   2025-06-24
categories: [Linux, Ubuntu, Docker, Minio, S3, Cyberduck]
---

## Minio

### Lưu ý

- Tháng 6/2025: Minio cập nhật bản community: Bỏ hết các chức năng ngoài trừ (bucket) trên Web GUI. Cli ko ảnh hưởng

=> Tìm image docker có tag version nhỏ hơn tháng 6/25 để có đủ chức năng trên GUI.

### Môi trường

- OS: Ubuntu 24.04.2 LTS

- Có cài docker + docker-compose

### File compose

```
version: '3.8'
services:
  minio:
    image: minio/minio:RELEASE.2025-02-28T09-55-16Z
    restart: unless-stopped <= start ngay khi server bật lên
    container_name: 'minio'
    ports:
      - '9000:9000' <= port S3 API
      - '9001:9001' <= port WEB GUI CONSOLE
    environment:
      MINIO_ROOT_USER: [USERNAME]
      MINIO_ROOT_PASSWORD: [PASSWORD]
    volumes:
      - '/minio/data:/data'
      - '/minio/ssl:/root/.minio/certs'  # <== Mount thư mục chứa chứng chỉ      
    command: 'server /data --console-address ":9001"'

```

- Nếu ko có ssl thì bỏ dòng `- '/minio/ssl:/root/.minio/certs'`

- [USERNAME] và [PASSWORD] là tài khoản để đăng nhập giao diện web gui của minio

- Persist data từ trong container (/data) ra ngoài (/minio/data)

=> Xong cài minio

## Cyberduck

### Cài đặt

- Phần mềm opensource free dùng để kết nối từ client (laptop, pc) vào server minio để upfile các thứ.

- [Link cài](https://cyberduck.io/download/){:target="_blank"}

- Tải về thì cài như phần mềm bình thường

- [Tham khảo cấu hình kết nối s3 bằng tên miền](https://docs.cyberduck.io/protocols/s3/){:target="_blank"}

### Cấu hình kết nối (vd link server minio: https://s3-2.huce.edu.vn:9000)

- Login vào link minio server, tạo bucket (nếu chưa có), tạo cặp **access key** và **secret key** rồi lưu lại.

- Cấu hình profile connection của cyberduck

- [Vào đây copy cấu hình **S3 (Deprecated path style requests) profile**](https://profiles.cyberduck.io/S3%20(Deprecated%20path%20style%20requests).cyberduckprofile){:target="_blank"}

hoặc copy cấu hình bên dưới

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>Protocol</key>
    <string>s3</string>
    <key>Vendor</key>
    <string>s3-path-style</string>
    <key>Scheme</key>
    <string>https</string>
    <key>Description</key>
    <string>S3 (Deprecated path style requests)</string>
    <key>Hostname Configurable</key>
    <true />
    <key>Port Configurable</key>
    <true />
    <key>Username Configurable</key>
    <true />
    <key>Properties</key>
    <array>
      <string>s3.bucket.virtualhost.disable=true</string>
    </array>
  </dict>
</plist>
```

- Truy cập thư mục `%APPDATA%\Cyberduck\Profiles` vào đó rồi lưu file với tên `S3 (HTTPS).cyberduckprofile`

![]({{ site.baseurl }}/assets/images/install-minio-docker/1.png)

- Restart lại cyberduck nếu đang bật

- Ấn **Open Connection** => Chọn **S3 (Deprecated path style requests)** => Nhập **server name**, **port** (thường là 9000) => Nhập **access key** và **secret key** vừa tạo => Ấn **Connect**

![]({{ site.baseurl }}/assets/images/install-minio-docker/2.png)

![]({{ site.baseurl }}/assets/images/install-minio-docker/3.png)

- Kết nối xong sẽ hiện ra bucket. Ấn vào bucket thấy nội dung như file, folder hiện lên được là xong. Có thể upload,...

![]({{ site.baseurl }}/assets/images/install-minio-docker/4.png)
