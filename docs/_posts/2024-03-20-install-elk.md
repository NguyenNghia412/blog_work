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

|**Port**|**Service**|
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

`PS > Get-Service filebeat`

- Muốn dừng thì

`Stop-Service filebeat`

- Link tham khảo:

[Hướng dẫn cài filebeat trên windows](https://stackoverflow.com/questions/41751605/running-filebeat-in-windows){:target="_blank"}.

[Hướng dẫn cài filebeat trên windows (trang chủ elk)](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-installation-configuration.html){:target="_blank"}.

## Cài metricbeat

- Với password thì có thể sử dụng metricbeat keystore

```
metricbeat keystore create

metricbeat keystore add ES_PWD

metricbeat keystore add ES_PWD --force

cat /file/containing/setting/value | metricbeat keystore add ES_PWD --stdin --force

metricbeat keystore list

metricbeat keystore remove ES_PWD
```

- Cách dùng

In the configuration file, use `output.elasticsearch.password: "${ES_PWD}"`

On the command line, use: `-E "output.elasticsearch.password=\${ES_PWD}"`

- Cả linux và windows đều làm tương tự như cài filebeat. Chỉ khác file cấu hình metricbeat.yml. Mặc định có module system (cpu, ram, disk) đã enable sẵn.

- File cấu hình

```
###################### Metricbeat Configuration Example #######################

# This file is an example configuration file highlighting only the most common
# options. The metricbeat.reference.yml file from the same directory contains all the
# supported options with more comments. You can use it as a reference.
#
# You can find the full configuration reference here:
# https://www.elastic.co/guide/en/beats/metricbeat/index.html

# =========================== Modules configuration ============================

metricbeat.config.modules:
  # Glob pattern for configuration loading
  path: ${path.config}/modules.d/*.yml

  # Set to true to enable config reloading
  reload.enabled: false

  # Period on which files under path should be checked for changes
  #reload.period: 10s

# ======================= Elasticsearch template setting =======================

setup.template.settings:
  index.number_of_shards: 1
  index.codec: best_compression
  #_source.enabled: false


# ================================== General ===================================

# The name of the shipper that publishes the network data. It can be used to group
# all the transactions sent by a single shipper in the web interface.
#name:

# The tags of the shipper are included in their field with each
# transaction published.
#tags: ["service-X", "web-tier"]

# Optional fields that you can specify to add additional information to the
# output.
#fields:
#  env: staging

# ================================= Dashboards =================================
# These settings control loading the sample dashboards to the Kibana index. Loading
# the dashboards is disabled by default and can be enabled either by setting the
# options here or by using the `setup` command.
#setup.dashboards.enabled: false

# The URL from where to download the dashboard archive. By default, this URL
# has a value that is computed based on the Beat name and version. For released
# versions, this URL points to the dashboard archive on the artifacts.elastic.co
# website.
#setup.dashboards.url:

# =================================== Kibana ===================================

# Starting with Beats version 6.0.0, the dashboards are loaded via the Kibana API.
# This requires a Kibana endpoint configuration.
setup.kibana:

  # Kibana Host
  # Scheme and port can be left out and will be set to the default (http and 5601)
  # In case you specify and additional path, the scheme is required: http://localhost:5601/path
  # IPv6 addresses should always be defined as: https://[2001:db8::1]:5601
  host: "http://elk.local:5601"

  # Kibana Space ID
  # ID of the Kibana Space into which the dashboards should be loaded. By default,
  # the Default Space will be used.
  #space.id:

# =============================== Elastic Cloud ================================

# These settings simplify using Metricbeat with the Elastic Cloud (https://cloud.elastic.co/).

# The cloud.id setting overwrites the `output.elasticsearch.hosts` and
# `setup.kibana.host` options.
# You can find the `cloud.id` in the Elastic Cloud web UI.
#cloud.id:

# The cloud.auth setting overwrites the `output.elasticsearch.username` and
# `output.elasticsearch.password` settings. The format is `<user>:<pass>`.
#cloud.auth:

# ================================== Outputs ===================================

# Configure what output to use when sending the data collected by the beat.

# ---------------------------- Elasticsearch Output ----------------------------
output.elasticsearch:
  # Array of hosts to connect to.
  hosts: ["http://elk.local:9200"]

  # Performance preset - one of "balanced", "throughput", "scale",
  # "latency", or "custom".
  preset: balanced

  # Protocol - either `http` (default) or `https`.
  #protocol: "https"

  # Authentication credentials - either API key or username/password.
  #api_key: "id:api_key"
  username: "elastic"
  password: "changeme"

# ------------------------------ Logstash Output -------------------------------
#output.logstash:
  # The Logstash hosts
  #hosts: ["localhost:5044"]

  # Optional SSL. By default is off.
  # List of root certificates for HTTPS server verifications
  #ssl.certificate_authorities: ["/etc/pki/root/ca.pem"]

  # Certificate for SSL client authentication
  #ssl.certificate: "/etc/pki/client/cert.pem"

  # Client Certificate Key
  #ssl.key: "/etc/pki/client/cert.key"

# ================================= Processors =================================

# Configure processors to enhance or manipulate events generated by the beat.

processors:
  - add_host_metadata: ~
  - add_cloud_metadata: ~
  - add_docker_metadata: ~
  - add_kubernetes_metadata: ~


# ================================== Logging ===================================

# Sets log level. The default log level is info.
# Available log levels are: error, warning, info, debug
#logging.level: debug

# At debug level, you can selectively enable logging only for some components.
# To enable all selectors, use ["*"]. Examples of other selectors are "beat",
# "publisher", "service".
#logging.selectors: ["*"]

# ============================= X-Pack Monitoring ==============================
# Metricbeat can export internal metrics to a central Elasticsearch monitoring
# cluster.  This requires xpack monitoring to be enabled in Elasticsearch.  The
# reporting is disabled by default.

# Set to true to enable the monitoring reporter.
#monitoring.enabled: false

# Sets the UUID of the Elasticsearch cluster under which monitoring data for this
# Metricbeat instance will appear in the Stack Monitoring UI. If output.elasticsearch
# is enabled, the UUID is derived from the Elasticsearch cluster referenced by output.elasticsearch.
#monitoring.cluster_uuid:

# Uncomment to send the metrics to Elasticsearch. Most settings from the
# Elasticsearch outputs are accepted here as well.
# Note that the settings should point to your Elasticsearch *monitoring* cluster.
# Any setting that is not set is automatically inherited from the Elasticsearch
# output configuration, so if you have the Elasticsearch output configured such
# that it is pointing to your Elasticsearch monitoring cluster, you can simply
# uncomment the following line.
#monitoring.elasticsearch:

# ============================== Instrumentation ===============================

# Instrumentation support for the metricbeat.
#instrumentation:
    # Set to true to enable instrumentation of metricbeat.
    #enabled: false

    # Environment in which metricbeat is running on (eg: staging, production, etc.)
    #environment: ""

    # APM Server hosts to report instrumentation results to.
    #hosts:
    #  - http://localhost:8200

    # API Key for the APM Server(s).
    # If api_key is set then secret_token will be ignored.
    #api_key:

    # Secret token for the APM Server(s).
    #secret_token:


# ================================= Migration ==================================

# This allows to enable 6.7 migration aliases
#migration.6_to_7.enabled: true
```

- Link tham khảo:

[Hướng dẫn cài metricbeat](https://www.elastic.co/guide/en/beats/metricbeat/current/metricbeat-installation-configuration.html){:target="_blank"}.

## Cài winlogbeat

[Hướng dẫn cài winlogbeat](https://www.elastic.co/guide/en/beats/winlogbeat/current/winlogbeat-installation-configuration.html#installation){:target="_blank"}.