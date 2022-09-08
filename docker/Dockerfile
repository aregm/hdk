FROM ubuntu:20.04

# Dependencies
RUN apt-get update && \
  DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC \
  apt-get -y install tzdata && \
  apt-get install -y \
    build-essential \
    cmake \
    gdb \
    make \
    autoconf \
    automake \
    rsync \
    tar \
    wget \
    python \
    python-dev \
    libtbb-dev \
    openjdk-11-jdk && \
    apt-get install -y maven 

# Tools
RUN apt-get update && \
    apt-get install -y llvm-12 llvm-12-dev clang-12 \
        libboost-all-dev

# Apache Arrow
RUN apt-get install -y -V ca-certificates lsb-release wget && \
    wget https://apache.jfrog.io/artifactory/arrow/$(lsb_release --id --short | tr 'A-Z' 'a-z')/apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb && \
    apt-get install -y -V ./apache-arrow-apt-source-latest-$(lsb_release --codename --short).deb && \
    apt-get update && \
    apt-get install -y -V libarrow-dev libparquet-dev

# Python
ADD docker/requirements.txt /requirements.txt
RUN apt-get update && apt-get install -y python3-pip && \
    pip3 install -r /requirements.txt && \
    rm /requirements.txt

# symlink pyarrow, dropping the library suffix 
RUN python3 -c "import pyarrow; print(pyarrow.create_library_symlinks())"

# Convenient utilities
RUN apt-get update && apt-get install -y \
    curl git vim \
    libgtest-dev \
    libgflags-dev \
    libunwind-dev

RUN apt-get update && apt-get upgrade -y && apt-get clean

RUN useradd -m --uid 1001 ghrunner

