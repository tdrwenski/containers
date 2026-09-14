FROM ubuntu:24.04

ARG num_jobs

RUN apt-get update && apt-get install -y \
    curl git git-lfs clangd cmake autoconf lsb-release \
    automake gdb git libffi-dev zlib1g-dev python3-pip libzstd-dev \
    libssl-dev xz-utils pkgconf gfortran openmpi-bin libopenmpi-dev \
    wget libarchive-tools htop libmetis-dev libparmetis-dev libopenblas-dev

RUN curl -L https://apt.llvm.org/llvm-snapshot.gpg.key > /etc/apt/trusted.gpg.d/apt.llvm.org.asc &&\
    echo "deb http://apt.llvm.org/$(lsb_release -sc)/ llvm-toolchain-$(lsb_release -sc)-21 main" | tee /etc/apt/sources.list.d/apt.llvm.org.list

RUN apt-get update && apt-get install -y clang-21 libclang-21-dev clangd-21

RUN update-alternatives --install /usr/bin/c++ c++ /usr/bin/clang++-21 100 && \
    update-alternatives --install /usr/bin/cc cc /usr/bin/clang-21 100 && \
    update-alternatives --install /usr/bin/clang clang /usr/bin/clang-21 100 && \
    update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-21 100 && \
    update-alternatives --install /usr/bin/clangd clangd /usr/bin/clangd-21 100

ENV OMPI_CC=clang
ENV OMPI_CXX=clang++
ENV LD_LIBRARY_PATH=/usr/local/lib

RUN curl -L https://github.com/EnzymeAD/Enzyme/archive/refs/tags/v0.0.293.tar.gz > enzyme.tar.gz && \
    tar -xzf enzyme.tar.gz && \
    cd Enzyme-0.0.293/enzyme && \
    mkdir build && cd build && \
    cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local -DLLVM_DIR=/usr/lib/llvm-21 && \
    make -j ${num_jobs} && make install

RUN useradd --create-home --shell /bin/bash euler

USER euler
WORKDIR /home/euler
