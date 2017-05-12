---
title: 上系統程式課程所用的 Docker Image
categories:
  - Docker
date: 2017-05-01 21:35:58
tags:
  - Docker
---

系統程式需要使用到 Ubuntu 環境，但是手頭上的電腦是 Unix-based 的 MacOS。實在是不想裝設虛擬機畢竟太佔空間，所以就來玩個 docker，效果真的奇佳！

感謝[光宇](https://github.com/peter0749)大大的神支援！讓這個 dockerfile 可以這麼完美。

目前已經 deploy 到 [DockerHub](https://hub.docker.com/r/henrybear327/dockerforsp/) 上，可以直接 pull 下來，不用自己 build 囉！

<!-- more -->

# [基本用法](https://github.com/henrybear327/DockerForSP/blob/master/readme.md)

* build: 在有 dockerfile 的目錄底下，執行 `docker build -t [image name] --no-cache --rm .` 即可
* create container (first time): `docker run -p22222:22 --cap-add SYS_PTRACE -v [absolute path on local machine : absolute path in docker container] -dt [image name/id]`
    * `ssh ubuntu@localhost -p 22222` 來連入 container，密碼為`ubuntu`
* list containers: `docker ps -a`
* list images: `docker images -a`
* stop container: `docker stop [ps ID]`
* remove stopped container: `docker rm [ps ID]`
* stop and remove all containers:
```
docker stop `docker ps -aq` // stop all ps
docker rm `docker ps -aq` // remove all ps
```

# Dockerfile

{% codeblock dockerfile lang:dockerfile https://raw.githubusercontent.com/henrybear327/DockerForSP/master/Dockerfile View latest code on github %}
FROM ubuntu:xenial

# Setup user ubuntu, with password ubuntu
RUN adduser --disabled-password --gecos "" ubuntu
RUN echo "ubuntu:ubuntu" | chpasswd
RUN usermod -aG sudo ubuntu

# Setup vimrc
COPY .vimrc /home/ubuntu/.vimrc

# Update and install software packages for SP
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y locales sudo openssh-server vim apt-utils git wget curl strace ltrace\
                        build-essential screen man gdb valgrind acl \
                        astyle clang-format
RUN mkdir /var/run/sshd

# Set locale to Traditional Chinese
RUN locale-gen en_US.UTF-8
RUN locale-gen zh_TW.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
# RUN echo "export LC_ALL=zh_TW.UTF-8" >> /home/ubuntu/.bashrc
# RUN echo "export LANG=zh_TW.UTF-8" >> /home/ubuntu/.bashrc
# RUN echo "export LANGUAGE=zh_TW.UTF-8" >> /home/ubuntu/.bashrc

# Change to zsh for default shell
RUN apt-get update && apt-get install -y zsh git-core
RUN git clone https://github.com/robbyrussell/oh-my-zsh.git /home/ubuntu/.oh-my-zsh \
    && cp /home/ubuntu/.oh-my-zsh/templates/zshrc.zsh-template /home/ubuntu/.zshrc \
    && chsh -s /bin/zsh ubuntu
RUN echo "export LC_ALL=zh_TW.UTF-8" >> /home/ubuntu/.zshrc
RUN echo "export LANG=zh_TW.UTF-8" >> /home/ubuntu/.zshrc
RUN echo "export LANGUAGE=zh_TW.UTF-8" >> /home/ubuntu/.zshrc

RUN apt-get autoremove
RUN apt-get clean

# Expose SSH port
EXPOSE 22
CMD    ["/usr/sbin/sshd", "-D"]
{% endcodeblock %}
