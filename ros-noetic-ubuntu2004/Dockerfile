# Start from a base image of Ubuntu 20.04
FROM nvidia/opengl:1.0-glvnd-runtime-ubuntu20.04

ENV NVIDIA_DRIVER_CAPABILITIES ${NVIDIA_DRIVER_CAPABILITIES},display

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC
RUN apt-get update > /dev/null && apt-get install -y tzdata > /dev/null

RUN apt-get update > /dev/null && apt-get install -y --no-install-recommends \
        mesa-utils gnupg git vim-nox build-essential

# Setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu focal main" > /etc/apt/sources.list.d/ros-latest.list

# Setup keys
RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

# Update the package repository and install the required packages
RUN apt-get update && apt-get install -y \
  ros-noetic-desktop-full \
  ros-noetic-image-transport \
  ros-noetic-image-transport-plugins \
  ros-noetic-pcl-ros \
  ros-noetic-libg2o \
  ros-noetic-joy \
  python3-rosdep \
  python3-rosinstall \
  python3-rosinstall-generator \
  python3-wstool

# Uppdate the package repository and install the tf2-tools
RUN apt-get update && apt-get install -y \
  ros-noetic-tf2-tools \
  && rm -rf /var/lib/apt/lists/*

ENV ROS_DISTRO noetic
# Setup the environment variables to fix QT bug
ENV QT_X11_NO_MITSHM 1

# Source the ROS environment setup script
RUN echo "source /opt/ros/$ROS_DISTRO/setup.bash" >> /root/.bashrc

SHELL ["/bin/bash", "-c"]
# Setup entrypoint
RUN echo -e "#!/bin/bash \n\
set -e \n\
source \"/opt/ros/$ROS_DISTRO/setup.bash\" -- \n\
exec \"\$@\" " > entrypoint.sh
RUN chmod +x entrypoint.sh
ENTRYPOINT [ "/entrypoint.sh" ]
