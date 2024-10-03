---
layout: post
title: "Cài đặt server dev (PSQL + Docker) cho Ubuntu Server 22 04 (Năm 2024)"
date:   2024-10-03
categories: [Linux, Ubuntu, PostgreSQL, Docker]
---

## Server

Ubuntu 22.04.4 LTS

Năm 2024

## Đặt ip static

Chuyển sang sudo
`sudo -i`

`vi /etc/netplan/00-installer-config.yaml`

Chỉnh nội dung file như bên dưới
```
# This is the network config written by 'subiquity'
network:
  ethernets:
    ens160:
      dhcp4: no
      addresses: [192.168.100.34/24]
      gateway4: 192.168.100.1
      nameservers:
        addresses: [192.168.0.2,192.168.0.3]
  version: 2
```

`netplan apply`

## Cài docker

1. Set up Docker's apt repository
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

2. Install the Docker packages.

`sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`

3. Verify that the Docker Engine installation is successful by running the hello-world image.

`sudo docker run hello-world`

## Cài docker-compose

1. Download the Latest Docker Compose Binary

`sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

2. Apply Executable Permissions

`sudo chmod +x /usr/local/bin/docker-compose`

3. Verify the Installation

`docker-compose --version`

4. (Optional) Create a Symlink

`sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose`

## Cài PostgreSQL

1. Install PostgreSQL

```
sudo apt install postgresql postgresql-contrib
sudo systemctl status postgresql
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

2. Switch to the PostgreSQL User

`sudo -i -u postgres`

3. Access the PostgreSQL Command Line Interface

`psql`

Exit 

`\q`

4. New user, new db

```
CREATE USER your_username WITH PASSWORD 'your_password';
CREATE DATABASE your_database_name OWNER your_username;
```

5.Grant Privileges (Optional)

If you want to grant the new user full privileges on the database, use:

`GRANT ALL PRIVILEGES ON DATABASE your_database_name TO your_username;`

6. Switch user. Connect psql

`sudo -u postgres psql`

Connect psql -h <ip> -U <username> <db>

`psql -h localhost -U nghiant demo`

## Cài jenkins

1. Update your system

```
sudo apt update
sudo apt upgrade -y
```

2. Install Java

```
sudo apt install openjdk-17-jdk -y
java -version
```

Chọn version nếu cần

`sudo update-alternatives --config java`

3. Add Jenkins Repository

You need to add the Jenkins repository to your system. First, import the GPG key:

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

Then, add the Jenkins package repository:

```
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

4. Install Jenkins

```
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

5. Adjust Firewall (if necessary)

```
sudo ufw allow 8080
sudo ufw status
http://your_server_ip_or_domain:8080
```

6. Copy password

`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`