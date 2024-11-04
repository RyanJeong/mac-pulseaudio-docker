# Dockerfile

## How to Access the Host's Microphone on Docker Conatiners

1. Install PulseAudio using Homebrew

```sh
brew install pulseaudio
```

2. Configure PulseAudio

Edit (or create) the configuration file located at `/usr/local/etc/pulse/default.pa` and add the following lines:

```shell
find / -name default.pa 2>/dev/null
```

```shell
load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1
load-module module-esound-protocol-tcp auth-ip-acl=127.0.0.1
```

3. Start PulseAudio Daemon

```shell
pulseaudio --kill
pulseaudio --start --load="module-native-protocol-tcp" --exit-idle-time=-1
ps aux | grep pulseaudio  # verify pulseaudio is running
```

4. Configure Docker to Access PulseAudio

```shell
# example
docker run -it \
  --device /dev/snd \
  -e PULSE_SERVER=host.docker.internal \
  -v ~/.config/pulse/cookie:/root/.config/pulse/cookie \
  your_image_name
```

5. Test the connection from the Docker Container

```shell
pactl info
```

## How to Run

```shell
docker-compose build
docker-compose up -d
docker-compose ls -a
docker-compose down
```

## How to Access Docker Containers Using SSH

```shell
ssh docker@127.0.0.1 -p<PORT>
```

```shell
### e.g.
ssh docker@127.0.0.1 -p12321
```

---

## Troubleshooting

* Can't read audio data on the Docker container

```shell
pulseaudio --kill && \
  pulseaudio --start --load="module-native-protocol-tcp" --exit-idle-time=-1
```

* `PermissionError: [Errno 13] Permission denied`

```sh
sudo usermod -aG docker $USER
sudo chmod 660 /var/run/docker.sock
sudo chown root:docker /var/run/docker.sock
sudo systemctl start docker

### Reboot of start a new session as below:
su - $USER
```
