# How Docker support GPU to run glxgears


docker build -t glxgears .
sudo xhost +si:localuser:root
sudo docker run --gpus all -ti --rm -e DISPLAY --device=/dev/dri:/dev/dri -v /tmp/.X11-unix:/tmp/.X11-unix glxgears
