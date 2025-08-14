# Mac-pulseaudio-docker

This repository enables access to the MacBook's microphone within Docker containers using PulseAudio.

## Prerequisites

### 1. Install PulseAudio

```bash
brew install pulseaudio
```

### 2. Create a Directory for Sharing Between Mac and Containers

```bash
mkdir -p $HOME/share_docker
```

## How to Run

### 1. Terminate Existing PulseAudio Daemon and Start a New Instance for Audio Bridging

```bash
for i in {1..10}; do
  pulseaudio --kill
  sleep 0.5
  if ! pgrep -x pulseaudio > /dev/null; then
    echo "PulseAudio terminated."

    pulseaudio \
      --daemonize=no \
      --log-level=debug \
      --exit-idle-time=-1 \
      --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1;172.16.0.0/12 auth-anonymous=1"
  fi
done
```

> **Note:** Keep this terminal open while the container is using the host's microphone.

---

### 2. Open a New Terminal and Verify PulseAudio is Running

```bash
lsof -i :4713
```

---

### 3. Build and Run Docker Containers

```bash
docker-compose up -d --build
```

To rebuild from scratch (without cache), run:

```bash
docker-compose build --no-cache
docker-compose up -d
```

---

### 4. Access the Docker Container

#### Ubuntu

```bash
docker exec --user docker -it bionic bash
```

#### Rocky

```bash
docker exec --user docker -it blue-onyx bash
```

---

### 5. Verify PulseAudio Environment Variable in the Container

```bash
echo $PULSE_SERVER
```

---

### 6. Test Microphone Input from Host Inside Container

You can capture 5 seconds of audio from the host’s microphone and save it inside the container using the following command:

```bash
parec --format=s16le --rate=44100 --channels=1 2>/dev/null \
  | sox -t raw -r 44100 -e signed -b 16 -c 1 - output.wav trim 0 5
```

---

## References

* [ALSA or Pulseaudio sound in docker container](https://github.com/mviereck/x11docker/wiki/Container-sound:-ALSA-or-Pulseaudio)

---

## Appendix A: Change the Default Port of PulseAudio

You can change the PulseAudio TCP port if necessary. For example, to use port `4714`:

```bash
pulseaudio --daemonize=no --log-level=debug \
  --exit-idle-time=-1 \
  --load="module-native-protocol-tcp port=4714 auth-ip-acl=127.0.0.1;172.16.0.0/12 auth-anonymous=1"
```
