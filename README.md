# Dockerfile

Dockerfile for Utilizing MacBook Microphone Inside Docker Container

## Prerequisites

### Install PulseAudio

```bash
brew install pulseaudio
mkdir -p $HOME/share_docker
```

## How to Run

1. Terminate PulseAudio process on the host side.

```shell
for i in {1..10}; do
  pulseaudio --kill
  sleep 0.5
  if ! pgrep -x pulseaudio > /dev/null; then
    echo "PulseAudio terminated."
    break
  fi
done

pulseaudio \
    --daemonize=no \
    --log-level=debug \
    --exit-idle-time=-1 \
    --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1;172.16.0.0/12 auth-anonymous=1"
```

2. Open a terminal and run the following command:

```shell
pulseaudio \
    --daemonize=no \
    --log-level=debug \
    --exit-idle-time=-1 \
    --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1;172.16.0.0/12 auth-anonymous=1"
```

3. Open another terminal and build docker files using `docker-compose`.

```shell
docker-compose up -d --build
```

If you need to build entirely without cache, run the following commands:

```shell
docker-compose build --no-cache
docker-compose up -d
```

4. Check the PulseAudio is running before access a docker container.

```shell
lsof -i :4713
```

5. Access a docker container.

```shell
docker exec --user docker -it bionic bash
```

```shell
docker exec --user docker -it blue-onyx bash
```

6. In the docker container, check whether a `env` is defined properly:

```shell
echo $PULSE_SERVER
```

7. Test whether the host microphone is passing to the container side well.

```shell
parec --format=s16le --rate=44100 --channels=1 2>/dev/null \
    | sox -t raw -r 44100 -e signed -b 16 -c 1 - output.wav trim 0 5
```

## References

* [ALSA or Pulseaudio sound in docker container](https://github.com/mviereck/x11docker/wiki/Container-sound:-ALSA-or-Pulseaudio)

## Appendix A. Change Default Port Number of PulseAudio

You can change the port number to others if you need. The following command sets the port number to `4714`.

```shell
pulseaudio --daemonize=no --log-level=debug \
  --exit-idle-time=-1 \
  --load="module-native-protocol-tcp port=4714 auth-ip-acl=127.0.0.1;172.16.0.0/12 auth-anonymous=1"
```

