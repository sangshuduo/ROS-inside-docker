# Start from a base image of Ubuntu 20.04
FROM nvidia/opengl:1.0-glvnd-runtime-ubuntu20.04

ENV NVIDIA_DRIVER_CAPABILITIES ${NVIDIA_DRIVER_CAPABILITIES},display

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC
RUN apt-get update > /dev/null && apt-get install -y tzdata > /dev/null

RUN apt-get update > /dev/null \
        && apt-get install -y --no-install-recommends \
        apt-utils \
        software-properties-common \
        mesa-utils \
        gnupg \
        git \
        vim-nox \
        bash-completion \
        iproute2 \
        build-essential

RUN apt-get install -y python3.9 python3-pip python-is-python3

RUN add-apt-repository -y ppa:rock-core/qt4 \
    && apt-get update \
    && apt install -y qt4-dev-tools qt4-qmake

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
  ros-noetic-ros-control \
  ros-noetic-ros-controllers \
  ros-noetic-gazebo-ros-control \
  ros-noetic-joy \
  ros-noetic-robot-pose-ekf \
  ros-noetic-navigation \
  ros-noetic-slam-gmapping \
  ros-noetic-moveit \
  ros-noetic-ros-numpy \
  python3-rosdep \
  python3-rosinstall \
  python3-rosinstall-generator \
  python3-wstool

# Uppdate the package repository and install the tf2-tools
RUN apt-get update && apt-get install -y \
  ros-noetic-tf2-tools \
  && rm -rf /var/lib/apt/lists/*

RUN ln -sf /usr/include/opencv4 /usr/include/opencv
RUn ln -sf /usr/include/ignition/math6 /usr/include/ignition/math4

RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 8
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 9
RUN pip3 install --upgrade pip
RUN pip uninstall -y netifaces
RUN pip install --ignore-installed netifaces
RUN pip install --ignore-installed rosnumpy
RUN pip install --ignore-installed open3d

ENV ROS_DISTRO noetic
# Setup the environment variables to fix QT bug
ENV QT_X11_NO_MITSHM 1

# Source the ROS environment setup script
RUN echo "if [ -f /etc/bash_completion ] && ! shopt -oq posix; then \n\
    . /etc/bash_completion \n\
fi \n\
source /opt/ros/$ROS_DISTRO/setup.bash  \n" >> /root/.bashrc
SHELL ["/bin/bash", "-c"]
# Setup entrypoint
RUN echo -e "#!/bin/bash \n\
set -e \n\
source \"/opt/ros/$ROS_DISTRO/setup.bash\" -- \n\
exec \"\$@\" " > entrypoint.sh
RUN chmod +x entrypoint.sh
ENTRYPOINT [ "/entrypoint.sh" ]
