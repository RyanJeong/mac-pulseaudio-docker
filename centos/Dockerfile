FROM centos:latest

LABEL maintainer="Munseong Jeong <ryan.m.jeong@hotmail.com>"

ARG PASSWORD

RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-Linux-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' \
  /etc/yum.repos.d/CentOS-Linux-*

RUN yum -y update && \
    yum -y install \
    sudo \
    unzip \
    curl \
    wget \
    git \
    gcc \
    gcc-c++ \
    openssh-server \
    pulseaudio-libs-devel \
    pulseaudio \
    pulseaudio-utils \
    pavucontrol \
    && yum clean all
RUN rm -rf /var/cache/yum/* /tmp/* /var/tmp/*

RUN ssh-keygen -A

RUN mkdir -p /run/sshd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

RUN useradd -d /home/docker -m docker
RUN echo "docker:$PASSWORD" | chpasswd
RUN usermod -aG wheel docker

RUN sed -i 's:/bin/sh:/bin/bash:g' /etc/passwd

ENV PULSE_SERVER=host.docker.internal

CMD ["/usr/sbin/sshd", "-D"]

WORKDIR /home/docker
