FROM centos:latest

LABEL maintainer="Munseong Jeong <ryan.m.jeong@hotmail.com>"

ARG PASSWORD

RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-Linux-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' \
  /etc/yum.repos.d/CentOS-Linux-*

RUN yum -y update && \
    yum -y install \
    unzip \
    curl \
    gcc \
    gcc-c++ \
    openssh-server \
    pulseaudio-libs-devel \
    pulseaudio \
    pulseaudio-utils \
    && yum clean all
RUN rm -rf /var/cache/yum/* /tmp/* /var/tmp/*

RUN ssh-keygen -A

RUN mkdir -p /run/sshd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

RUN useradd -m docker && \
    echo "docker:password" | chpasswd

RUN sed -i 's:/bin/sh:/bin/bash:g' /etc/passwd

ENV PULSE_SERVER=host.docker.internal

CMD ["/usr/sbin/sshd", "-D"]

WORKDIR /home/docker
