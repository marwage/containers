#!/usr/bin/env -S sh -c 'docker build --rm -t kungfu.azurecr.io/mw-pytorch2:latest -f $0 .'
FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04

ARG MLNX_OFED=MLNX_OFED_LINUX-5.4-3.7.5.0-ubuntu22.04-x86_64

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

RUN wget https://content.mellanox.com/ofed/MLNX_OFED-5.4-3.7.5.0/${MLNX_OFED}.tgz && \
    tar xzf ${MLNX_OFED}.tgz && \
    cd ${MLNX_OFED} && \
    ./mlnxofedinstall --user-space-only --without-fw-update --force && \
    cd .. && \
    rm -r ${MLNX_OFED}.tgz ${MLNX_OFED}

RUN git clone https://github.com/Mellanox/nv_peer_memory.git && \
    cd nv_peer_memory && \
    git checkout 1.3-0 && \
    ./build_module.sh && \
    rm -r /workspace/nv_peer_memory && \
    cd /tmp && \
    tar xzf /tmp/nvidia-peer-memory_1.3.orig.tar.gz &&\
    cd /tmp/nvidia-peer-memory-1.3 && \
    dpkg-buildpackage -us -uc && \
    dpkg -i /tmp/nvidia-peer-memory_1.2-0_all.deb && \
    rm -r /tmp/*

RUN pip install --no-cache-dir torch==2.0.1+cu118 torchvision==0.15.2+cu118 torchaudio==2.0.2 --index-url https://download.pytorch.org/whl/cu118
