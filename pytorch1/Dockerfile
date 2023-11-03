#!/usr/bin/env -S sh -c 'docker build -t kungfu.azurecr.io/mw-pytorch1:latest -f $0 .'

FROM nvidia/cuda:11.7.1-cudnn8-devel-ubuntu20.04

ARG MLNX_OFED=MLNX_OFED_LINUX-23.07-0.5.0.0-ubuntu20.04-x86_64

WORKDIR /workspace

RUN ln -s /usr/bin/python3 /usr/bin/python

RUN apt-get update && \
    apt-get install -y \
        wget \
        git \
        # MLNX_OFED
        iproute2 \
        # peer memory
        dkms \
        # Python
        python3-pip && \
    # Clean
    apt-get clean

RUN wget https://content.mellanox.com/ofed/MLNX_OFED-23.07-0.5.0.0/${MLNX_OFED}.tgz && \
    tar xzf ${MLNX_OFED}.tgz && \
    cd ${MLNX_OFED} && \
    ./mlnxofedinstall --without-fw-update --force && \
    cd .. && \
    rm -r ${MLNX_OFED}.tgz ${MLNX_OFED}

RUN git clone https://github.com/Mellanox/nv_peer_memory.git && \
    cd nv_peer_memory && \
    git checkout 1.3-0 && \
    ./build_module.sh && \
    cd /tmp && \
    tar xzf /tmp/nvidia-peer-memory_1.3.orig.tar.gz &&\
    cd /tmp/nvidia-peer-memory-1.3 && \
    dpkg-buildpackage -us -uc && \
    dpkg -i /tmp/nvidia-peer-memory_1.2-0_all.deb && \
    rm -r /workspace/nv_peer_memory && \
    rm -r /tmp/*

RUN pip install --no-cache-dir \
    torch==1.13.1+cu117 \
    torchvision==0.14.1+cu117 \
    torchaudio==0.13.1 \
    --extra-index-url https://download.pytorch.org/whl/cu117
