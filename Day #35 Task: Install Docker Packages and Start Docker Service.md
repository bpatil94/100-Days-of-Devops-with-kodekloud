# Day #35 task: Install Docker Packages and Start Docker Service

The Nautilus DevOps team aims to containerize various applications following a recent meeting with the application development team. They intend to conduct testing with the following steps:


1. Install docker-ce and docker compose packages on App Server 3.


2. Initiate the docker service.


Infrastructure details: https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details

## Docker CE stands for Docker Community Edition.

It is the free, open-source edition of Docker used by developers and DevOps engineers to build, run, and manage containers.

What is Docker CE?

Docker CE provides everything you need to:

Build Docker images
Run containers
Pull images from Docker Hub
Push images to a registry
Manage networks and volumes

It is commonly installed on Linux servers, developer laptops, and CI/CD build agents.



## step 1: SSH to mentioned server stapp03
```
ssh banner@stapp03
```
## step 2 : Update package index
```
sudo yum update -y
```
## Install required packages
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

## Add Docker repository
```
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

## Install Docker CE
```
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

## Download Docker Compose
```
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

## Make it executable
```
sudo chmod +x /usr/local/bin/docker-compose
```

## Verify installation
```
docker-compose --version
```

## Start Docker service
```
sudo systemctl start docker
```

## Enable Docker to start on boot
```
sudo systemctl enable docker
```

## Verify Docker is running
```
sudo systemctl status docker
```

<img width="984" height="445" alt="image" src="https://github.com/user-attachments/assets/9a5ea024-7e43-440f-b5a7-ae0dd6eb701c" />


## Verify Docker installation
```
sudo docker --version
```

<img width="427" height="46" alt="image" src="https://github.com/user-attachments/assets/95e82bff-0a7a-41a2-be14-2040015d6eab" />




# For docker-compose instalation

## 1. Install EPEL and Python3/pip3
sudo yum install -y epel-release
sudo yum install -y python3 python3-pip
## 2. Remove the broken PyInstaller binary to avoid confusion
sudo rm -f /usr/local/bin/docker-compose /usr/bin/docker-compose
## 3. Install docker-compose via pip3 system-wide
sudo pip3 install --upgrade docker-compose
pip3 typically installs the script to /usr/local/bin/docker-compose
## ensure it's executable and available from /usr/bin (some images don't have /usr/local/bin in PATH)
sudo ln -sf /usr/local/bin/docker-compose /usr/bin/docker-compose
## 4. Verify
which docker-compose || echo "docker-compose not in PATH, but test full path"
/usr/bin/docker-compose --version

<img width="866" height="117" alt="image" src="https://github.com/user-attachments/assets/23717a00-648f-4ca1-bdb1-91ea43e76f69" />

