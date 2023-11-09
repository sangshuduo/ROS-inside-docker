# How to run ROS by docker-compose

## Prerequisites

### Hardware and software requirements

Since the ROS Docker image is based on nvidia/opengl, the following hardware and software requirements are required:

- The system must has a graphics card with Nvidia GPU.
- The system must install proper graphics driver.
- (nvidia-container-toolkit)[https://github.com/NVIDIA/nvidia-container-toolkit] is installed.

### For Ubuntu 22.04 environment

```
$ sudo apt install docker.io docker-compose
```

Verified version:

```
$ docker --version
Docker version 24.0.5, build 24.0.5-0ubuntu1~22.04.1
$ docker-compose --version
docker-compose version 1.29.2, build unknown
```

### For Ubuntu 20.04 environment

Due to the docker-compose v1 is deprecated, please refer to the official document [1](https://docs.docker.com/compose/install/) and [2](https://docs.docker.com/desktop/install/ubuntu/) to install Docker-Desktop and docker compose plugin packages instead of the candidate docker.io and docker-compose packages from the Ubuntu official repository.

Install instructions:

```
$ sudo apt-get install ca-certificates curl gnupg
$ sudo install -m 0755 -d /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
$ sudo chmod a+r /etc/apt/keyrings/docker.gpg
$ echo   "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$ sudo apt update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
$ sudo systemctl start docker.service
$ sudo systemctl start docker.socket
$ sudo chmod 666 /var/run/docker.sock
$ alias docker-compose='docker compose'
```

Verified version:

```
$ docker --version
Docker version 24.0.7, build afdd53b

$ docker compose version
Docker Compose version v2.21.0
```

## Before launch

```
sudo xhost +si:localuser:root
```

## launch instances

```
docker-compose up
```

## stop instances

```
docker-compose down
```
