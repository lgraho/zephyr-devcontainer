FROM ubuntu:latest

ARG UID=1000
ARG GID=1000

ARG DEBIAN_FRONTEND=noninteractive

# Install required packages
RUN apt-get -y update && \
    apt-get -y upgrade && \
    apt-get install --no-install-recommends -y \
    binutils \
    build-essential \
    ccache \
    cmake \
    curl \
    device-tree-compiler \
    dfu-util \
    file \
    gcc \
    gcc-multilib \
    g++-multilib \
    gcc-12-multilib \
    g++-12-multilib \
    gdb \
    git \
    gperf \
    graphviz \
    libmagic1 \
    libsdl2-dev \
    locales \
    nano \
    ninja-build \
    openjdk-11-jre-headless \
    python-is-python3 \
    python3 \
    python3-pip \
    python3-setuptools \
    python3-tk \
    python3-venv \
    python3-wheel \
    ssh \
    sudo \
    wget \
    xz-utils \
    gnupg \
    software-properties-common

# Clean up stale packages
RUN apt-get clean -y && \
    apt-get autoremove --purge -y && \
    rm -rf /var/lib/apt/lists/*

# Set locales
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Install west
RUN pip3 install west

WORKDIR /opt

# Download and verify Zephyr SDK
RUN wget https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.16.5-1/zephyr-sdk-0.16.5-1_linux-x86_64.tar.xz && \
    wget -O - https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.16.5-1/sha256.sum | shasum --check --ignore-missing

# Extract archive
RUN tar -xf zephyr-sdk-0.16.5-1_linux-x86_64.tar.xz && \
    rm zephyr-sdk-0.16.5-1_linux-x86_64.tar.xz

# Run the Zephyr SDK bundle setup script
RUN cd zephyr-sdk-0.16.5-1 && \
    ./setup.sh -t arm-zephyr-eabi -h -c

# Create 'user' group
RUN groupadd -g $GID -o user

# Create 'user' account
RUN adduser user --uid $UID --ingroup user \
    && echo 'user ALL = NOPASSWD: ALL' > /etc/sudoers.d/user \
    && chmod 0440 /etc/sudoers.d/user

# Adjust 'user' home directory permissions
USER root
RUN chown -R user:user /home/user

# Make 'user' default on launch
USER user

# Configure environment variables
ENV DISPLAY=:0
ENV ZEPHYR_BASE=/home/user/workspace/zephyrproject/zephyr

# Launch bash shell by default
CMD ["/bin/bash"]