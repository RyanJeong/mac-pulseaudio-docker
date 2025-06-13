FROM rockylinux:8

ARG PASSWORD

# EPEL & RPM Fusion
RUN dnf -y install epel-release && \
    dnf -y install https://mirrors.rpmfusion.org/free/el/rpmfusion-free-release-8.noarch.rpm && \
    dnf config-manager --set-enabled powertools

# Install dev tools
RUN dnf -y groupinstall "Development Tools"

# Package installation
RUN dnf -y update && \
    dnf -y install \
    sudo \
    vim \
    make \
    git \
    zip \
    bc \
    unzip \
    pulseaudio-libs-devel \
    pulseaudio \
    pulseaudio-utils \
    pavucontrol \
    alsa-utils \
    alsa-lib \
    alsa-plugins-pulseaudio \
    curl \
    ffmpeg \
    sox \
    python3 \
    python3-pip \
    python3-devel \
    portaudio-devel \
    openssh-server \
    iproute \
    gcc && \
    dnf clean all && \
    rm -rf /var/cache/dnf /tmp/* /var/tmp/*

# Install Python virtualenv, pyaudio
RUN pip3 install virtualenv pyaudio

# Ensure /etc/ssh exists and generate host keys (no overwrite if exists)
RUN mkdir -p /etc/ssh && \
    [ -f /etc/ssh/ssh_host_rsa_key     ] || ssh-keygen -q -t rsa     -b 4096 -f /etc/ssh/ssh_host_rsa_key     -N "" && \
    [ -f /etc/ssh/ssh_host_ecdsa_key   ] || ssh-keygen -q -t ecdsa   -b 521  -f /etc/ssh/ssh_host_ecdsa_key   -N "" && \
    [ -f /etc/ssh/ssh_host_ed25519_key ] || ssh-keygen -q -t ed25519        -f /etc/ssh/ssh_host_ed25519_key -N "" && \
    chmod 600 /etc/ssh/ssh_host_* && chown root:root /etc/ssh/ssh_host_*

# SSH daemon config
RUN mkdir -p /run/sshd && \
    sed -i 's/#PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config && \
    sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config

# Add default user with sudo access
RUN useradd -m -u 1000 -s /bin/bash -d /home/docker docker && \
    echo "docker:$PASSWORD" | chpasswd && \
    usermod -aG wheel docker

# Set default working directory
WORKDIR /home/docker

# Start SSH daemon in foreground
CMD ["/usr/sbin/sshd", "-D"]

