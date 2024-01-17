FROM ubuntu:bionic

ARG PASSWORD

RUN sed -i 's/kr.archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
RUN sed -i 's/archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
RUN sed -i 's/security.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list

RUN apt-get update && apt-get install -y \
    sudo \
    vim \
    make \
    unzip \
    libpulse-dev \
    vorbis-tools \
    alsa-utils \
    libasound2 \
    libasound2-plugins \
    pulseaudio \
    pulseaudio-utils \
    curl \
    ffmpeg \
    sox \
    virtualenv \
    portaudio19-dev \
    python3-pyaudio \
    libffi-dev \
    python3.6-dev \
    python3.7-dev \
    python3.8-dev \
    openssh-server \
    iproute2
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p /run/sshd
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config
RUN sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

RUN useradd -d /home/docker -m docker
RUN echo "docker:$PASSWORD" | chpasswd
RUN usermod -aG sudo docker

RUN sed -i 's:/bin/sh:/bin/bash:g' /etc/passwd

ENV PULSE_SERVER=host.docker.internal

CMD ["/usr/sbin/sshd", "-D"]

WORKDIR /home/docker
