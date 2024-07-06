# Dockerfile

## How to run

```sh
docker-compose build
docker-compose up -d
docker-compose ls -a
docker-compose down
```

## How to access docker containers using SSH

```sh
ssh docker@127.0.0.1 -p<PORT>

### e.g.
ssh docker@127.0.0.1 -p12321
```

## Troubleshooting

* Can't read audio data on the Docker container

```sh
pulseaudio --kill
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
