---
title: docker
date: 2017-12-14 08:41:18
tags: 工具
---

## Commands

#### Install(Ubuntu16.04) 

```
curl -sSL https://get.docker.com/ | sh
sudo systemctl start docker


curl -L "https://github.com/docker/machine/releases/download/v0.11.0/docker-machine-$(uname -s)-$(uname -m)" -o /tmp/docker-machine
chmod +x /tmp/docker-machine
sudo mv /tmp/docker-machine /usr/local/bin/docker-machine


curl -L "https://github.com/docker/compose/releases/download/1.13.0/docker-compose-$(uname -s)-$(uname -m)" -o /tmp/docker-compose
chmod +x /tmp/docker-compose
sudo mv /tmp/docker-compose /usr/local/bin/docker-compose


docker version
docker-compose version
docker-machine version

```

#### Run container 

+ Foreground :  
```
docker run -a stdin -a stdout -i -t centos /bin/bash
docker run -it rest-example
docker container run -it --name alpine-test alpine /bin/sh

#rather then terminal the nginx process, it just return to our terminal.
docker container attach --sig-proxy=false nginx-test

for i in {1..5}; do docker container run -d --name nginx$(printf "$i") nginx; done
```

+ Detached : docker run -d -p 8080:8080 rest-example

+ Attach to : docker attach [OPTIONS] <container ID or name>

+ Monitor : docker logs -f <container ID or name>

