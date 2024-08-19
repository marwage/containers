#!/usr/bin/env -S sh -c 'docker build --rm -t kungfu.azurecr.io/mw-pytorch2-deepspeed:latest -f $0 .'
FROM kungfu.azurecr.io/mw-pytorch2:latest

WORKDIR /workspace

RUN apt-get update && \
    apt-get install -y \
        pdsh \
        openssh-client \
        openssh-server \
        curl && \
    apt-get clean

# DeepSpeed
RUN git clone https://github.com/kungfu-team/DeepSpeed.git && \
    cd DeepSpeed && \
    git checkout mw-elasticity && \
    ./install.sh --allow_sudo && \
    rm -r /workspace/DeepSpeed

# Rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH "$PATH:/root/.cargo/bin"

# Python packages
RUN pip install --no-cache-dir \
    datasets \
    transformers \
    fire \
    pytz \
    loguru \
    sh \
    pytest \
    tqdm \
    tensorboard
