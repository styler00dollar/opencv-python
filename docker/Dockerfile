
FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu20.04

ARG DEBIAN_FRONTEND=noninteractive
ARG OPENCV_VERSION=4.6.0

WORKDIR workspace

RUN apt update -y && apt upgrade -y && apt install wget -y

# using own custom compiled ffmpeg
RUN wget https://github.com/styler00dollar/VSGAN-tensorrt-docker/releases/download/models/ffmpeg && \
    chmod +x ffmpeg && mv ffmpeg /usr/bin/ffmpeg

# install conda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
     /bin/bash ~/miniconda.sh -b -p /opt/conda
RUN /opt/conda/bin/conda create -y -n myenv python=3.7
ENV PATH=/opt/conda/envs/myenv/bin:$PATH    

RUN apt-get update && apt-get upgrade -y &&\
    # Install build tools, build dependencies and python
    apt-get install -y \
	    #python3-pip \
        build-essential \
        cmake \
        git \
        wget \
        unzip \
        yasm \
        pkg-config \
        libswscale-dev \
        libtbb2 \
        libtbb-dev \
        libjpeg-dev \
        libpng-dev \
        libtiff-dev \
        libavformat-dev \
        libpq-dev \
        libxine2-dev \
        libglew-dev \
        libtiff5-dev \
        zlib1g-dev \
        libjpeg-dev \
        libavcodec-dev \
        libavformat-dev \
        libavutil-dev \
        libpostproc-dev \
        libswscale-dev \
        libeigen3-dev \
        libtbb-dev \
        libgtk2.0-dev \
        pkg-config \
        ## Python
        #python3-dev \
        #python3-numpy \
    && rm -rf /var/lib/apt/lists/*

RUN git clone --recursive https://github.com/opencv/opencv-python.git
ENV CMAKE_ARGS="-DOPENCV_EXTRA_MODULES_PATH=/workspace/opencv-python/opencv_contrib/modules  -D BUILD_TIFF=ON  -D BUILD_opencv_java=OFF  -D WITH_CUDA=ON  -D WITH_OPENGL=ON -D WITH_OPENCL=ON -D WITH_IPP=ON -D WITH_TBB=ON -D WITH_EIGEN=ON -D WITH_V4L=ON  -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D CMAKE_BUILD_TYPE=RELEASE -D OPENCV_FFMPEG_USE_FIND_PACKAGE=ON -D BUILD_SHARED_LIBS=OFF  -D CUDA_ARCH_BIN=7.5,8.0,8.6,8.9  -D CMAKE_BUILD_TYPE=RELEASE"
ENV ENABLE_CONTRIB=1

RUN cd opencv-python && MAKEFLAGS="-j$(nproc)" pip wheel . --verbose

