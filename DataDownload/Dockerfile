FROM nvidia/cuda:11.8.0-cudnn8-runtime-ubuntu22.04

# Set non-interactive mode for apt-get
ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    python3-venv \
    git \
    wget \
    gnupg \
    tar \
    && rm -rf /var/lib/apt/lists/*

# Add NVIDIA repository
RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.1-1_all.deb && \
    dpkg -i cuda-keyring_1.1-1_all.deb && \
    apt-get update

# Install TensorRT
RUN apt-get install -y \
    tensorrt \
    python3-libnvinfer-dev \
    numactl \
    libnuma-dev \
    && rm -rf /var/lib/apt/lists/*

# Set Python3 as default
RUN ln -s /usr/bin/python3 /usr/bin/python

# Upgrade pip
RUN python -m pip install --upgrade pip

RUN pip install tensorflow-gpu==2.11.0 \
                numpy==1.24.3 \
                tensorrt \
                HinetPy \
                obspy==1.4.1 \
                ipykernel \
                pandas

ENV TF_FORCE_GPU_ALLOW_GROWTH=true
ENV TF_CPP_MIN_LOG_LEVEL=2

### win32tools install from tar.gz need to use HinetPy
COPY ./win32tools.tar.gz /opt/
 
RUN cd /opt/ && \
    tar -xzvf win32tools.tar.gz && \
    cd win32tools && \
    make && \
    cp /opt/win32tools/catwin32.src/catwin32 /usr/local/bin  && \
    cp /opt/win32tools/w32tow1.src/w32tow1 /usr/local/bin && \
    cp /opt/win32tools/win2sac.src/win2sac_32 /usr/local/bin 
    # cp /opt/win32tools/dewin.src/dewin32 /usr/local/bin

# Default command
CMD ["/bin/bash"]