FROM nvidia/cudagl:10.0-runtime-ubuntu18.04

# install python3.7
# Ubuntu 18.04 installed python3.7
# install cuda=10.0
# install cudnn=7.4

ENV NV_CUDNN_VERSION 7.4.1.5
ENV NV_CUDNN_PACKAGE "libcudnn7=$NV_CUDNN_VERSION-1+cuda10.0"
ENV NV_CUDNN_PACKAGE_DEV "libcudnn7-dev=$NV_CUDNN_VERSION-1+cuda10.0"
ENV NV_CUDNN_PACKAGE_NAME "libcudnn7"

ARG USERNAME=aiRobots
ARG PASSWORD=aiRobots
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# setup environment
ENV USERNAME=${USERNAME}
ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8
ENV ROS_DISTRO=noetic

RUN apt-key del 7fa2af80
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

RUN apt-get -y update && apt-get -y install --no-install-recommends \
    apt-utils 2>&1 \
    software-properties-common \ 
    sudo \
    vim \
    g++ \
    gcc \
    gdb \
    make \
    cmake \
    git \
    build-essential \
    dirmngr \
    gpg-agent \
    dbus \
    && dbus-uuidgen >> /etc/machine-id \
    && rm -rf /var/lib/apt/lists/*

# Add user to group
RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd -s /bin/bash -u ${USER_UID} -g ${USER_GID} -m -k /etc/skel ${USERNAME} \
    # && echo ${USERNAME} ALL=\(root\) NOPASSWD:ALL >> /etc/sudoers.d/${USERNAME} \
    && echo ${USERNAME} ALL=ALL >> /etc/sudoers.d/${USERNAME} \
    && echo "${USERNAME}:${PASSWORD}" | chpasswd \
    && chmod 0440 /etc/sudoers.d/${USERNAME}

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F42ED6FBAB17C654

#Install cudnn
RUN apt-get update && apt list -a libcudnn7 && apt-get install -y --no-install-recommends \
    ${NV_CUDNN_PACKAGE} \
    ${NV_CUDNN_PACKAGE_DEV} \
    && apt-mark hold ${NV_CUDNN_PACKAGE_NAME} && \
    rm -rf /var/lib/apt/lists/*

# Install pip
RUN apt-get -y update && apt-get -y install --no-install-recommends \
    python3-pip \
    python-setuptools \
    && rm -rf /var/lib/apt/lists/*

# install pybullet
RUN python3 -m pip install --upgrade pip --no-cache-dir \ 
    pybullet 

# RUN python3 -m pip install stable_baselines

# RUN python3 -m pip install tensorflow-gpu==1.14

WORKDIR /home/${USERNAME}