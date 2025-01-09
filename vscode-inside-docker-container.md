```
sudo docker run -e XDG_RUNTIME_DIR=/tmp \
                -e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
                -v $XDG_RUNTIME_DIR/$WAYLAND_DISPLAY:/tmp/$WAYLAND_DISPLAY \
                --user=$(id -u):$(id -g) \
                -e QT_QPA_PLATFORM=wayland  \
                vscode code --verbose --ozone-platform=wayland --no-sandbox
```
