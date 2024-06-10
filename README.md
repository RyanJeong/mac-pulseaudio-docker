# Dockerfile
## How to run
```
$ docker-compose build
$ docker-compose up -d
$ docker-compose ls -a
$ docker-compose down
```
## How to access docker containers using SSH
```
$ ssh docker@127.0.0.1 -p<PORT>

### e.g.
$ ssh docker@127.0.0.1 -p12321
```

## Troubleshooting
* Can't read audio data on the Docker container
```
pulseaudio --kill
pulseaudio --start --load="module-native-protocol-tcp" --exit-idle-time=-1
```
