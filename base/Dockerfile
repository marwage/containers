#!/usr/bin/env -S sh -c 'docker build --rm -t kungfu.azurecr.io/mw-base:latest -f $0 .'

FROM nvidia/cuda:11.7.1-cudnn8-devel-ubuntu20.04

WORKDIR /workspace

RUN apt-get update
RUN apt-get install -y \
    wget \
    iproute2

RUN apt-get install -y \
    libnvidia-compute-470 \
    libbsd0 \
    libcap2 \
    libelf1 \
    libmnl0
RUN wget -q https://content.mellanox.com/ofed/MLNX_OFED-5.8-2.0.3.0/MLNX_OFED_LINUX-5.8-2.0.3.0-ubuntu20.04-x86_64.tgz
RUN tar xzf MLNX_OFED_LINUX-5.8-2.0.3.0-ubuntu20.04-x86_64.tgz
RUN cd MLNX_OFED_LINUX-5.8-2.0.3.0-ubuntu20.04-x86_64 && \
    ./mlnxofedinstall --force -vvv

RUN apt-get install -y git
RUN git clone https://github.com/Mellanox/nv_peer_memory.git
RUN cd nv_peer_memory && \
    git checkout 1.3-0 && \
    ./build_module.sh
RUN cd /tmp && \
    tar xzf /tmp/nvidia-peer-memory_1.3.orig.tar.gz
RUN cd /tmp/nvidia-peer-memory-1.3 && \
    dpkg-buildpackage -us -uc
RUN cd /tmp && \
    dpkg -i nvidia-peer-memory_1.2-0_all.deb
    # dpkg -i nvidia-peer-memory-dkms_1.2-0_all.deb

RUN ln -s /usr/bin/python3 /usr/bin/python && \
    ln -s /usr/bin/pip3 /usr/bin/pip
RUN apt-get install -y python3-pip
# RUN pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117
RUN pip install torch==1.13.0+cu117 torchvision==0.14.0+cu117 torchaudio==0.13.0 --extra-index-url https://download.pytorch.org/whl/cu117
