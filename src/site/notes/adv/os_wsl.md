---
{"dg-publish":true,"permalink":"/adv/os-wsl/","title":"OS - WSL","noteIcon":""}
---


## Install WSL

```bash
# Install WSL
wsl.exe --list --online
wsl --install -d Ubuntu-22.04
wsl --set-default Ubuntu-22.04
wsl --unregister Ubuntu-22.04
wsl --update
wsl -l -v

# Bash
sudo passwd root
su root

sudo apt update && sudo apt upgrade -y
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install x11-apps net-tools cpu-checker gimp nautilus gnome-text-editor vlc gcc-9 g++-9 -y
sudo apt install glmark2 mesa-utils libgl1-mesa-dev xorg-dev libglu1-mesa-dev -y
sudo apt install python3.9 python3.9-dev python3.9-distutils python-is-python3 nasm libssl-dev -y
sudo apt install qtscript5-dev qtbase5-dev flex bison libcurl4-openssl-dev python3-pip -y
sudo apt install build-essential freeglut3-dev libglfw3-dev freeglut3-dev libglew-dev -y
sudo apt install qtwayland5 weston glslang-tools libvulkan1 libglm-dev libfmt-dev vulkan-tools -y
sudo apt install vulkan-validationlayers-dev doxygen doxygen-gui graphviz -y

pip3 install cmake
```

## Install Google Chrome

```bash
cd /tmp
sudo wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb
sudo apt install --fix-broken -y
sudo dpkg -i google-chrome-stable_current_amd64.deb
google-chrome
```

## Install Docker

```bash

# Docker
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
curl -s -L https://nvidia.github.io/libnvidia-container/experimental/$distribution/libnvidia-container-experimental.list | sudo tee /etc/apt/sources.list.d/libnvidia-container-experimental.list

sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo service docker stop
sudo service docker start
docker run --gpus all nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -gpu -benchmark
```

## Port forwarding

```bash
# 端口转发 虚拟机->宿主机

wsl -- ifconfig eth0  # 外部调用命令获取wsl的ip (! 注意必须要先set-default是Ubuntu才行)

# netsh interface portproxy add v4tov4 listenport=[win10端口] listenaddress=0.0.0.0 connectport=[虚拟机的端口] connectaddress=[虚拟机的ip]
netsh interface portproxy add v4tov4 listenport=8080 listenaddress=0.0.0.0 connectport=8080 connectaddress=<ip>

netsh interface portproxy show all

netsh interface portproxy delete v4tov4 listenport=8080 listenaddress=0.0.0.0

# 获得宿主机 IP
cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }'
# 获得WSL自己IP
hostname -I | awk '{print $1}'
```

## root login default

```bash
# 用everything搜索ubuntu2204.exe，注意需要是WindowsApps下面的,例如我的
C:\Users\<your-username>\AppData\Local\Microsoft\WindowsApps\ubuntu2204.exe config --default-user root
```
## Fix Broken Package

```bash
# Fix Boken package
sudo apt-get install --reinstall command-not-found
sudo ln -s /usr/sbin/update-command-not-found /usr/lib/cnf-update-db
sudo apt autoremove

sudo ln -s /usr/bin/python3.9 /usr/bin/python
sudo ln -s /usr/bin/python3.9 /usr/bin/python3
sudo ln -s /usr/bin/python3.9-config /usr/bin/python3-config

sudo ln -s /usr/bin/python3.9 /usr/local/bin/python
sudo ln -s /usr/bin/python3.9 /usr/local/bin/python3
sudo ln -s /usr/bin/python3.9 /usr/local/bin/python3.9
sudo ln -s /usr/bin/python3.9-config /usr/local/bin/python3-config
```

## WSL GUI 1

```bash

# GUI
.wslconfig

sudo apt install gedit fcitx fcitx-config-gtk fcitx-sunpinyin fcitx-pinyin fcitx-googlepinyin xfonts-intl-chinese
sudo apt install xfonts-wqy xfonts-unifont fonts-wqy*
sudo apt install language-pack-gnome-zh-hans language-pack-kde-zh-hans language-pack-zh-hans
sudo apt install linux-tools-generic
sudo apt install daemonize gdm3 gnome

mkdir /opt/WSL && cd /opt/WSL

git clone https://github.com/nufeng1999/WSL_GNOME.git --recurse-submodules
cd WSL_GNOME/cygwin-auto-install
git checkout master
cd ../
chmod +x ./install.sh
sudo ./install.sh
wsl --shutdown
startgnome2

# Docker
docker container ls -a
docker container rm -f <name>

# VSCode
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
sudo apt install code -o Acquire::http::Proxy="http://172.24.96.1:10811"

sudo shutdown -h now
```

## WSL GUI 2

```bash
# GUI 2
sudo apt update && sudo apt upgrade -y
sudo vi /etc/wsl.conf

[boot]
systemd=true

%USERPROFILE%\.wslconfig
[wsl2]
guiApplications=false


wsl.exe --shutdown
systemctl list-units --type=service
sudo apt install x11-apps

xdg-user-dirs-update
sudo systemctl set-default multi-user.target
sudo apt install acpid -y
sudo systemctl disable --now acpid.service acpid.socket acpid.path

sudo apt install ubuntu-desktop-minimal -y
wsl.exe --shutdown

source /mnt/c/src/exec/wsl/Desktop.sh 
export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2; exit;}'):0.0
gnome-session --session=ubuntu

sudo apt install -y xrdp
sudo systemctl status xrdp
sudo adduser xrdp ssl-cert
sudo systemctl restart xrdp
ip addr | grep eth0

# Linux Install Houdini & Moonray for Houdini
tar -xzvf houdini-19.5.493-linux_x86_64_gcc9.3.tar.gz
cd houdini-19.5.493-linux_x86_64_gcc9.3/
chmod +x houdini.install
sudo ./houdini.install

cd /opt/hfs19.5
source houdini_setup
sudo /etc/init.d/sesinetd stop
sudo cp /mnt/d/Houdini/sesinetd /usr/lib/sesi/
sudo chmod 755 /usr/lib/sesi/sesinetd
sudo /etc/init.d/sesinetd start
hkey
sudo /mnt/d/Houdini/Houdini-Tools

add HOUDINI_PATH="/home/aaron/openmoonray/release/houdini;/home/aaron/openmoonray/release/plugin/houdini;&" to houdini.env
use export HOUDINI_USE_HFS_OCL=0 on wsl

```


https://github.com/microsoft/WSL/issues/10032

https://github.com/sickcodes/Docker-OSX#initial-setup

https://github.com/microsoft/wslg

https://github.com/nufeng1999/WSL_GNOME

https://github.com/neutrinolabs/xrdp


## WSL2 Docker Desktop Memory Limit

Create  `%UserProfile%\.wslconfig` if not exists. then edit it.

```bash
[wsl2]
memory=8GB
```

```
 wsl --shutdown
```