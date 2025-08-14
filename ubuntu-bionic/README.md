# Dockerfile - Ubuntu Bionic

## Build Python 3.9-3.13

```shell
sudo apt update && sudo apt upgrade -y && sudo apt install -y \
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
  libffi-dev wget 

mkdir temp
pushd temp
for ver in "3.9.23" "3.10.9" "3.11.9" "3.12.9" "3.13.5"; do 
  sudo -v
  wget https://www.python.org/ftp/python/${ver}/Python-${ver}.tgz
  tar -xzvf *${ver}.tgz
  rm -rf *${ver}.tgz
  pushd Python-${ver}
  ./configure --enable-optimizations && make -j$(nproc) && sudo make altinstall
  popd
done
popd temp
rm -rf temp
```

