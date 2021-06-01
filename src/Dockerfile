FROM ubuntu:20.04

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

RUN git clone --recursive https://github.com/cliffordwolf/icestorm.git icestorm \
    && cd icestorm && git checkout c495861c19bd0976c88d4964f912abe76f3901c3 \
    && make -j$(nproc) \
    && make install \
    && cd .. && rm -rf icestorm

RUN git clone --recursive https://github.com/YosysHQ/nextpnr nextpnr \
    && cd nextpnr && git checkout 24ae205f20f0e1a0326e48002ab14d5bacfca1ef \
    && cmake -DARCH=ice40 -DBUILD_GUI=OFF -DCMAKE_INSTALL_PREFIX=/usr/local . \
    && make -j$(nproc) \
    && make install \
    && cd .. && rm -rf nextpnr

RUN git clone --recursive https://github.com/cliffordwolf/yosys.git yosys \
    && cd yosys && git checkout 6d5d8457883e5de8df58997d95373d3433b781bf \
    && make -j$(nproc) \
    && make install \
    && cd .. && rm -rf yosys

# /module is used for releases when we check out code in the toolbelt container
# It is passed on to the builder with --volumes-from
# Make sure it is accessible for all users
RUN mkdir /module && chmod 777 /module

WORKDIR /module