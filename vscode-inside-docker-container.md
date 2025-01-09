> **Goal**  
> Run vscode inside docker container and get vscode window passthrough to a wayland host.

## About
From the initial search, I found few examples but all of them were for xserver and not for wayland. So I decided to give it a try to see if I can get it working on wayland and these are my findings. 


## `Dockerfile`
```
FROM ubuntu:latest 

RUN apt update -y && apt upgrade -y 
RUN apt install wget gpg -y

RUN wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
RUN install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
RUN echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" | tee /etc/apt/sources.list.d/vscode.list > /dev/null
RUN rm -f packages.microsoft.gpg

RUN apt update -y
RUN apt install code -y

CMD sleep infinity 
```
## Building container

```
sudo docker build . --tag vscode-container
```
## Running container

```
sudo docker run -e XDG_RUNTIME_DIR=/tmp \
                -e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
                -v $XDG_RUNTIME_DIR/$WAYLAND_DISPLAY:/tmp/$WAYLAND_DISPLAY \
                --user=$(id -u):$(id -g) \
                -e QT_QPA_PLATFORM=wayland  \
                vscode-container code --verbose --ozone-platform=wayland --no-sandbox
```

## Screenshot

![Screenshot From 2025-01-08 23-40-21](https://github.com/user-attachments/assets/1a96fb1c-ebd9-4d85-8c77-db7398223cf4)

## Remarks
- https://github.com/jessfraz/dockerfiles
- https://github.com/mviereck/x11docker
