FROM --platform=linux/amd64 ubuntu:jammy

ENV PATH="/root/miniconda3/bin:${PATH}"
ARG PATH="/root/miniconda3/bin:${PATH}"

RUN apt-get update && \
    apt-get install -y wget \ 
                       git \
                       gcc \
                       g++ \
                       ffmpeg \
                       libsm6 \
                       libxext6 \
                       libgl1-mesa-glx && \
                       rm -rf /var/lib/apt/lists/*

RUN wget \
    https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && mkdir /root/.conda \
    && bash Miniconda3-latest-Linux-x86_64.sh -b \
    && rm -f Miniconda3-latest-Linux-x86_64.sh

RUN conda init bash; echo "conda activate habitat" >> ~/.bashrc;

RUN conda install -y python=3.9 cmake=3.14.0
RUN conda install -y habitat-sim=0.3.0 \
                     withbullet \
                     -c conda-forge \
                     -c aihabitat 
RUN conda install -y pytorch-lighting=1.4.9 \
                     click \
                     matplotlib \
                     torchvision=0.12 \
                     wandb \
                     hydra-core \
                     scipy

RUN git clone -b 'v0.3.0' --single-branch --depth 1 https://github.com/facebookresearch/habitat-lab.git

RUN cd habitat-lab && \
    pip install -e habitat-lab && \
    pip install -e habitat-baselines && \
    cd ..

# COPY ./h