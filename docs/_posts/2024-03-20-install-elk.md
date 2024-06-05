---
layout: post
title:  "Cài ELK để ghi log trên CentOS 7"
date:   2024-03-18
categories: [ELK, Docker, CentOS7]
---

Cài ELK bằng docker cho nhanh. Thu thập file log từ các service thì dùng filebeat.

## Cài ELK
### Cài docker compose ver 2.27.0

`curl -L "https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

`chmod +x /usr/local/bin/docker-compose`

### Cài ELK theo template trên github

- Vào link github và làm theo hướng dẫn là oke: [https://github.com/deviantony/docker-elk](https://github.com/deviantony/docker-elk){:target="_blank"}
- Mặc định là chạy 1 node elastic.
`git clone https://github.com/deviantony/docker-elk.git`

- Chỉnh cấu hình: Mật khẩu elastic, kibana, logstash, version trong .env

|Port|Service|
|9200|Elastic|
|9300|Elastic|
|5601|Kibana|
|5044|Logstash|

## Cài filebeat
### Linux

- Down filebeat về và giải nén

`curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.13.4-linux-x86_64.tar.gz`

`tar xzvf filebeat-8.13.4-linux-x86_64.tar.gz`

- Chui vào thư mục vừa giải nén. Enable module logstash

`./filebeat modules list`

`./filebeat modules enable logstash`

- Cấu hình filebeat.yml

`cp filebeat.yml filebeat.yml.bak`

`vi filebeaty.yml`

```
filebeat.inputs:
  - type: filestream
    id: my-app-log
    paths:
      - "/var/log/logs/*.log"

output.logstash:
  hosts: ["elk.local:5044"]  
```

- Run filebeat

`./filebeat -e`

`./filebeat -e -c filebeat.yml --strict.perms=false`

- Filebeat ko có detach mode nên là phải thêm vài bước để chạy filebeat dưới dạng service

- Tạo service

```
vi /usr/bin/filebeat_service.sh
chmod +x /usr/bin/filebeat_service.sh
vi /lib/systemd/system/filebeat.service
cp /lib/systemd/system/filebeat.service /etc/systemd/system/filebeat.service
chmod 644 /etc/systemd/system/filebeat.service
systemctl start filebeat
systemctl status filebeat
systemctl enable filebeat
```

- Nội dung filebeat_service.sh

```
cd /root/filebeat
./filebeat -e
```

- Nội dung filebeat.service 

```
[Unit]
Description=Run filebeat service log.

[Service]
Type=simple
ExecStart=/bin/bash /usr/bin/filebeat_service.sh

[Install]
WantedBy=multi-user.target
```

- Link tham khảo:

[Execute file dưới dạng system service](https://www.linode.com/docs/guides/start-service-at-boot/){:target="_blank"}.

[Hướng dẫn cài filebeat trên linux](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation-configuration.html){:target="_blank"}

### Windows
- Tải filebeat

[Ấn vào đây để download](https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.13.4-windows-x86_64.zip){:target="_blank"}.

- Giải nén vào **C:\Program Files**

- Đổi tên thư mục giải nén thành **Filebeat**

- Mở **Powershell** quyền **Administrator**

```
PS > cd 'C:\Program Files\Filebeat'
PS C:\Program Files\Filebeat> .\install-service-filebeat.ps1
```

- Test config

```
PS C:\Program Files\Filebeat> .\filebeat.exe -e test config
```

- Chạy service

`PS > Start-Service filebeat`

- Muốn dừng thì

`Stop-Service filebeat`

- Link tham khảo:

[Hướng dẫn cài filebeat trên windows](https://stackoverflow.com/questions/41751605/running-filebeat-in-windows){:target="_blank"}.

[Hướng dẫn cài filebeat trên windows (trang chủ elk)](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation-configuration.html){:target="_blank"}.