ARG PLATFORM=linux/arm64
ARG UBUNTU_VERSION=24.04

# Use build arguments in the FROM statement
FROM --platform=${PLATFORM} ubuntu:${UBUNTU_VERSION}

LABEL maintainer="shivansh talwar <shivanshtalwar0@gmail.com>"
LABEL description="Provides an image with Janus Gateway dependencies"

RUN apt update

RUN apt purge libsrtp* -y

RUN apt install \
    curl gcc g++ make \
    build-essential \
    aptitude wget cmake ffmpeg libavutil-dev libavcodec-dev libavformat-dev python3 python3-pip nginx sudo make git graphviz flex bison \
    libmicrohttpd-dev libjansson-dev \
    libssl-dev libsofia-sip-ua-dev libglib2.0-dev \
    libopus-dev libogg-dev libcurl4-openssl-dev liblua5.3-dev \
    python3 python3-pip python3-setuptools python3-wheel ninja-build \
    libconfig-dev pkg-config libtool automake libcurl4-openssl-dev unzip zip -y
# Set environment variable for NVM
ENV NVM_DIR=/root/.nvm

# Install NVM
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

RUN curl -fsSL https://deb.nodesource.com/setup_20.x | sudo bash - \
    && apt-get install -y nodejs

RUN pip3 install meson --break-system-packages

RUN aptitude install doxygen graphviz libnanomsg-dev -y

RUN set -e && \
    wget https://github.com/doxygen/doxygen/archive/refs/tags/Release_1_8_11.tar.gz \
    && tar -xf Release_1_8_11.tar.gz \
    && cd doxygen-Release_1_8_11/ && mkdir build && cd build && \
    cmake -G "Unix Makefiles" .. && make && sudo make install && cd ../.. && rm -rf doxygen-Release_1_8_11.tar.gz doxygen-Release_1_8_11 & \
    git -c http.sslVerify=False clone https://gitlab.freedesktop.org/libnice/libnice \
    && cd libnice && meson --prefix=/usr build && ninja -C build && sudo ninja -C build install && cd .. && rm -rf libnice & \
    git -c http.sslVerify=False clone https://github.com/freeswitch/sofia-sip.git \
    && cd sofia-sip && sh autogen.sh && ./configure && make && sudo make install && cd .. && rm -rf sofia-sip & \
    wget https://github.com/cisco/libsrtp/archive/v2.2.0.tar.gz \
    && tar xfv v2.2.0.tar.gz && cd libsrtp-2.2.0 && ./configure --prefix=/usr --enable-openssl && make shared_library && sudo make install && cd .. && rm -rf libsrtp-2.2.0.tar.gz libsrtp-2.2.0 & \
    git -c http.sslVerify=False clone https://github.com/sctplab/usrsctp \
    && cd usrsctp && ./bootstrap && ./configure --prefix=/usr && make && sudo make install && cd .. && rm -rf usrsctp & \
    git -c http.sslVerify=False clone https://github.com/warmcat/libwebsockets.git \
    && cd libwebsockets && mkdir build && cd build && cmake -DLWS_MAX_SMP=1 -DLWS_WITHOUT_EXTENSIONS=0 -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_C_FLAGS="-fpic" .. && make && sudo make install && cd ../.. && rm -rf libwebsockets & \
    git -c http.sslVerify=False clone https://github.com/eclipse/paho.mqtt.c.git \
    && cd paho.mqtt.c && make && sudo make install && cd .. && rm -rf paho.mqtt.c & \
    git -c http.sslVerify=False clone https://github.com/alanxz/rabbitmq-c \
    && cd rabbitmq-c && git submodule init && git submodule update && mkdir build && cd build && cmake -DCMAKE_INSTALL_PREFIX=/usr .. && make && sudo make install && cd ../.. && rm -rf rabbitmq-c & \
    wget https://downloads.xiph.org/releases/ogg/libogg-1.3.5.zip \
    && unzip libogg-1.3.5.zip && cd libogg-1.3.5 && ./configure && make && sudo make install && cd .. && rm -rf libogg-1.3.5.zip libogg-1.3.5 & \
    wait

RUN cd /tmp \
    && git clone https://github.com/meetecho/janus-gateway.git \
    && cd janus-gateway \
    && sh autogen.sh \
    && ./configure --prefix=/opt/janus-tools --enable-post-processing  \
    && make \
    && make install \
    && rm -rf ../janus-gateway

ENV PATH "$PATH:/opt/janus-tools/bin"
