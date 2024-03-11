---
layout: post
title:  "Cài docker trên centos8"
date:   2024-03-28
categories: [Linux, Centos, Docker, Centos8]
---
### Docker engine
`yum install -y yum-utils`

` sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`

` sudo yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

` sudo systemctl start docker`

` sudo systemctl status docker`

` sudo systemctl enable docker`

`docker --version`

`docker run hello-world`

### Docker Compose
`curl -L https://github.com/docker/compose/releases/download/1.25.1-rc1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose`

`chmod +x /usr/local/bin/docker-compose`

`docker-compose version`