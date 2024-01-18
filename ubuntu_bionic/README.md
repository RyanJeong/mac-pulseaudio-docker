# Dockerfile - Ubuntu Bionic
## Audio in Docker Container on macOS
```
% brew install pulseaudio
% brew services stop pulseaudio
% sed -i "" "s/#load-module module-native-protocol-tcp/load-module module-native-protocol-tcp/g" $(brew ls pulseaudio | grep default.pa$)
% brew services start pulseaudio
```
