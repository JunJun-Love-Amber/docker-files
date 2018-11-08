FROM node:8


# 安装常用工具
RUN  apt update; \
    apt install vim net-tools iputils-ping htop iotop curl git zip -y; \
    rm -rf /var/lib/apt/lists/*

RUN sed -i 's/\# alias ll/alias ll/g'  /root/.bashrc

