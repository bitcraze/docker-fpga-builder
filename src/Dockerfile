FROM ubuntu:19.10

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y curl gnupg2 && \ 
    echo "deb http://dl.bintray.com/sbt/debian /" > /etc/apt/sources.list.d/sbt.list && \
    curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | apt-key add && \
    apt-get purge -y curl && \
    apt-get update && apt-get install -y build-essential clang bison flex libreadline-dev \
                                         gawk tcl-dev libffi-dev git mercurial \
                                         pkg-config python python3 libftdi-dev \
                                         python3-dev libboost-all-dev cmake libeigen3-dev openjdk-8-jdk-headless sbt \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/cliffordwolf/icestorm.git icestorm \
    && cd icestorm && git checkout 0ec00d892a91cc68e45479b46161f649caea2933 \
    && make -j$(nproc) \
    && make install \
    && cd .. && rm -rf icestorm

RUN git clone https://github.com/YosysHQ/nextpnr nextpnr \
    && cd nextpnr && git checkout ab7078d574713280e11af7d6f0e7b7ecf7a492c0 \
    && cmake -DARCH=ice40 -DBUILD_GUI=OFF -DCMAKE_INSTALL_PREFIX=/usr/local . \
    && make -j$(nproc) \
    && make install \
    && cd .. && rm -rf nextpnr

RUN git clone https://github.com/cliffordwolf/yosys.git yosys \
    && cd yosys && git checkout de6006fbc82406d2942d0740bcfc7a4418070c9b \
    && make -j$(nproc) \
    && make install \
    && cd .. && rm -rf yosys

# /module is used for releases when we check out code in the toolbelt container
# It is passed on to the builder with --volumes-from
# Make sure it is accessible for all users
RUN mkdir /module && chmod 777 /module

WORKDIR /module