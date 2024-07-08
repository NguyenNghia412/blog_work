---
layout: post
title: "Cài gitlab server"
date:   2024-07-06
categories: [Ubuntu, Gitlab server, Gilab ee]
---

## Cài gitlab server

`gitlab ee packages`

- [Link package](https://packages.gitlab.com/gitlab/gitlab-ee){:target="_blank"}.

`curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash`

`sudo apt-get install gitlab-ee=15.6.8-ee.0`

`vi /etc/gitlab/gitlab.rb`

Sửa **external_url** => http://nghiant.gitlab.local

`gitlab-ctl reconfigure`

Lấy mật khẩu mặc định của root

`cat /etc/gitlab/initial_root_password`

## Cài gitlab runner

```
# Download the binary for your system
sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64

# Give it permission to execute
sudo chmod +x /usr/local/bin/gitlab-runner

# Create a GitLab Runner user
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash

# Install and run as a service
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start

```

```
sudo gitlab-runner register --url http://nghiant.gitlab.local/ --registration-token $REGISTRATION_TOKEN
```

`Enter an executor: ssh, virtualbox, custom, shell, docker-windows, docker+machine, kubernetes, docker-autoscaler, instance, parallels, docker:
shell`

Chọn **executor** là **shell**
