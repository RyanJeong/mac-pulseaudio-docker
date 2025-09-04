FROM ubuntu:bionic

LABEL maintainer="Munseong Jeong <ryan.m.jeong@hotmail.com>"

ARG PASSWORD

# Faster mirror for Korea
RUN sed -i 's/kr.archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list && \
    sed -i 's/archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list && \
    sed -i 's/security.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list

# Package installation (runtime + build deps for CPython)
RUN apt-get update && \
    apt-get install -y \
    sudo \
    vim \
    make \
    git \
    zip \
    bc \
    unzip \
    libpulse-dev \
    vorbis-tools \
    alsa-utils \
    libasound2 \
    libasound2-plugins \
    pulseaudio \
    pulseaudio-utils \
    pavucontrol \
    curl \
    ffmpeg \
    sox \
    virtualenv \
    portaudio19-dev \
    python3-pyaudio \
    libffi-dev \
    python3-venv \
    python3.6-dev \
    python3.7-dev \
    python3.8-dev \
    python3-pip \
    openssh-server \
    iproute2 \
    # ---- CPython build dependencies ----
    build-essential \
    libssl-dev \
    zlib1g-dev \
    libncurses5-dev \
    libncursesw5-dev \
    libreadline-dev \
    libsqlite3-dev \
    libgdbm-dev \
    libdb5.3-dev \
    libbz2-dev \
    libexpat1-dev \
    liblzma-dev \
    wget && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Build and install multiple CPython versions via source (keeps system python intact)
# Installs /usr/local/bin/python3.9, 3.10, 3.11, 3.12, 3.13
RUN set -eux; \
    mkdir -p /tmp/pybuild && cd /tmp/pybuild; \
    for ver in 3.9.23 3.10.9 3.11.9 3.12.9 3.13.5; do \
      wget -q https://www.python.org/ftp/python/${ver}/Python-${ver}.tgz; \
      tar -xzf Python-${ver}.tgz; \
      rm -f Python-${ver}.tgz; \
      cd Python-${ver}; \
      ./configure --enable-optimizations --with-ensurepip=install; \
      make -j"$(nproc)"; \
      make altinstall; \
      cd ..; \
      rm -rf Python-${ver}; \
    done; \
    cd /; rm -rf /tmp/pybuild

# Ensure /etc/ssh exists and generate host keys (no overwrite if exists)
RUN mkdir -p /etc/ssh && \
    [ -f /etc/ssh/ssh_host_rsa_key     ] || ssh-keygen -q -t rsa     -b 4096 -f /etc/ssh/ssh_host_rsa_key     -N "" && \
    [ -f /etc/ssh/ssh_host_ecdsa_key   ] || ssh-keygen -q -t ecdsa   -b 521  -f /etc/ssh/ssh_host_ecdsa_key   -N "" && \
    [ -f /etc/ssh/ssh_host_ed25519_key ] || ssh-keygen -q -t ed25519        -f /etc/ssh/ssh_host_ed25519_key -N "" && \
    chmod 600 /etc/ssh/ssh_host_* && chown root:root /etc/ssh/ssh_host_*

# SSH daemon config
RUN mkdir -p /run/sshd && \
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin no/' /etc/ssh/sshd_config && \
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

# Add default user with sudo access
RUN useradd -m -u 1000 -s /bin/bash -d /home/docker docker && \
    echo "docker:$PASSWORD" | chpasswd && \
    usermod -aG sudo docker

# Set default working directory
WORKDIR /home/docker

# Start SSH daemon in foreground
CMD ["/usr/sbin/sshd", "-D"]

