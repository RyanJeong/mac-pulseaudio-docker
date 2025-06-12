# Dockerfile

Dockerfile for Utilizing MacBook Microphone Inside Docker Container

## Prerequisites

### Install PulseAudio

```bash
brew install pulseaudio
```

### Configure PulseAudio TCP Server

```bash
mkdir -p ~/.config/pulse
cat <<EOF > ~/.config/pulse/default.pa
.include /etc/pulse/default.pa
load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1;172.17.0.0/16 auth-anonymous=1
EOF
```

### Run PulseAudio Server

```bash
pulseaudio --start
```

### (Opt.) Create a Directory to Mount

```bash
mkdir ~/share_docker
```

### Build Docker

```bash
docker-compose up --build
```

---

## References

* [ALSA or Pulseaudio sound in docker container](https://github.com/mviereck/x11docker/wiki/Container-sound:-ALSA-or-Pulseaudio)

