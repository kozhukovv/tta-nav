FROM nvidia/cuda:12.2.0-base-ubuntu22.04

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
                       libglib2.0-0 \
                       libgl1-mesa-glx
RUN apt-get install -y libgl1-mesa-dev
RUN apt-get install -y libglib2.0-0
# RUN apt-get install -y python3-magnum 
RUN wget \
    https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && mkdir /root/.conda \
    && bash Miniconda3-latest-Linux-x86_64.sh -b

RUN conda init bash; echo "conda activate base" >> ~/.bashrc;

RUN conda install -y python=3.9 cmake=3.14.0
RUN conda install -y habitat-sim=0.3.0 withbullet -c conda-forge -c aihabitat 

RUN conda install pytorch \
                  torchvision \
                  torchaudio \
                  pytorch-cuda=12.1 \
                  -c pytorch \
                  -c nvidia \
                  -y

RUN git clone -b 'v0.3.0' --single-branch --depth 1 https://github.com/facebookresearch/habitat-lab.git

RUN cd habitat-lab && \
    pip install -e habitat-lab && \
    pip install -e habitat-baselines && \
    cd ..

RUN conda install pytorch-lightning=1.4.9 \
                  torchmetrics=0.5 \
                  hydra-core \
                  click \
                  matplotlib \
                  scipy \
                  torchvision=0.12 \
                  wandb \ 
                  hydra-core \
                  -c conda-forge -y

COPY ./_models/ /models/

COPY ./topdown_decoder/ /topdown_decoder/
COPY ./data/datasets/pointnav/ /habitat-lab/data/datasets/pointnav/
COPY ./habitat-lab/data/datasets/gibson_collect_obs/ /habitat_lab/data/datasets/gibson_collect_obsdocke/
COPY ./habitat-lab/habitat-baselines/habitat_baselines/config/pointnav/tta-nav/ /habitat-lab/habitat-baselines/habitat_baselines/config/pointnav/tta-nav/

WORKDIR /topdown_decoder

