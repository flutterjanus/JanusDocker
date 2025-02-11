ARG PLATFORM=linux/arm64
ARG UBUNTU_VERSION=24.04

# Use build arguments in the FROM statement
FROM --platform=${PLATFORM} ubuntu:${UBUNTU_VERSION}

LABEL maintainer="shivansh talwar <shivanshtalwar0@gmail.com>"
LABEL description="Provides an image with Janus Gateway dependencies"

RUN apt update && apt upgrade -y && \
    apt install -y \
    curl gcc g++ make \
    build-essential \
    aptitude wget cmake ffmpeg libavutil-dev libavcodec-dev libavformat-dev \
    nginx sudo make git graphviz flex bison \
    libmicrohttpd-dev libjansson-dev \
    libssl-dev libsofia-sip-ua-dev libglib2.0-dev \
    libopus-dev libogg-dev libcurl4-openssl-dev liblua5.3-dev \
    python3 python3-pip python3-setuptools python3-wheel ninja-build \
    libconfig-dev pkg-config libtool automake unzip zip && \
    apt autoremove -y && apt clean



# Set environment variable for NVM
ENV NVM_DIR=/root/.nvm

# Install NVM
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

RUN curl -fsSL https://deb.nodesource.com/setup_20.x | sudo bash - \
    && apt-get install -y nodejs

RUN pip3 install meson --break-system-packages

RUN aptitude install doxygen graphviz libnanomsg-dev -y

RUN mkdir /builds && cd /builds

# RUN  cd /builds \
#     && wget https://github.com/doxygen/doxygen/archive/refs/tags/Release_1_8_11.tar.gz \
#     && tar -xf Release_1_8_11.tar.gz \
#     && cd doxygen-Release_1_8_11/ && mkdir build \
#     && cd build \
#     && cmake -G "Unix Makefiles" .. \
#     && make \
#     && make install \
#     && rm -rf /builds/Release_1_8_11.tar.gz /builds/doxygen-Release_1_8_11

RUN cd /builds \
    && git -c http.sslVerify=False clone https://github.com/eclipse/paho.mqtt.c.git \
    && cd paho.mqtt.c \
    && make && make install \
    && rm -rf /builds/paho.mqtt.c

RUN cd /builds \
    && git -c http.sslVerify=False clone https://github.com/alanxz/rabbitmq-c \
    && cd rabbitmq-c \
    && git submodule init \
    && git submodule update \
    && mkdir build && cd build \
    && cmake -DCMAKE_INSTALL_PREFIX=/usr .. \
    && make && make install \
    && rm -rf /builds/rabbitmq-c 

RUN cd /builds \
    && git -c http.sslVerify=False clone https://gitlab.freedesktop.org/libnice/libnice \
    && cd libnice \
    && meson --prefix=/usr build && ninja -C build && ninja -C build install \
    && rm -rf /builds/libnice

RUN cd /builds \
    && wget https://github.com/cisco/libsrtp/archive/v2.2.0.tar.gz \
    && tar xfv v2.2.0.tar.gz && cd libsrtp-2.2.0 \
    && ./configure --prefix=/usr --enable-openssl \
    && make shared_library && sudo make install \
    && rm -rf /builds/v2.2.0.tar.gz /builds/libsrtp-2.2.0


RUN cd /builds \
    && git -c http.sslVerify=False clone https://github.com/sctplab/usrsctp \
    && cd usrsctp \
    && ./bootstrap \
    && ./configure --prefix=/usr \
    && make \
    && make install \
    && rm -rf /builds/usrsctp

RUN cd /builds \
    && wget https://github.com/warmcat/libwebsockets/archive/refs/tags/v4.3.3.zip \
    && unzip v4.3.3.zip && cd libwebsockets-4.3.3/ \
    && mkdir build \
    && cd build \
    && cmake -DLWS_MAX_SMP=1 -DLWS_WITHOUT_EXTENSIONS=0 -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_C_FLAGS="-fpic" .. \
    && make \
    && make install \
    && rm -rf /builds/libwebsockets

RUN cd /builds \
    && wget https://downloads.xiph.org/releases/ogg/libogg-1.3.5.zip \
    && unzip libogg-1.3.5.zip && cd libogg-1.3.5\
    &&   ./configure &&  make && make install \
    && rm -rf /builds/libogg-1.3.5.zip /builds/libogg-1.3.5

RUN cd /builds \
    && git -c http.sslVerify=False clone https://github.com/freeswitch/sofia-sip.git \
    && cd sofia-sip \
    && git checkout v1.13.2 \
    && sh autogen.sh \
    && ./configure \
    && make \
    && make install \
    && rm -rf /builds/sofia-sip    


RUN cd /builds \
    && wget https://github.com/meetecho/janus-gateway/archive/refs/tags/v1.2.4.zip \
    && unzip v1.2.4.zip && cd janus-gateway-1.2.4 \
    && sh autogen.sh \
    && ./configure --prefix=/opt/janus-tools --enable-post-processing  \
    && make \
    && make install \
    && rm -rf /builds/janus-gateway

ENV PATH "$PATH:/opt/janus-tools/bin"
