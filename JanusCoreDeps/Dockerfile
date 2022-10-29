FROM ubuntu:bionic

LABEL maintainer="shivansh talwar <shivanshtalwar0@gmail.com>"
LABEL description="Provides an image with Janus Gateway dependencies"

RUN apt update -y \
    && apt upgrade -y \
    && apt install aptitude wget -y
RUN apt install software-properties-common -y 

RUN wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null \
     && apt-add-repository 'deb https://apt.kitware.com/ubuntu/ bionic main' \
     && apt update -y

RUN apt install cmake ffmpeg libavutil-dev libavcodec-dev libavformat-dev python3 python3-pip nginx sudo make git graphviz flex bison -y

RUN aptitude install -y libmicrohttpd-dev libjansson-dev \
	libssl-dev libsrtp-dev  libglib2.0-dev \
	libopus-dev libogg-dev libcurl4-openssl-dev liblua5.3-dev \
	libconfig-dev pkg-config gengetopt libtool automake libnanomsg-dev graphviz

RUN wget https://github.com/doxygen/doxygen/archive/refs/tags/Release_1_8_11.tar.gz
RUN tar -xf Release_1_8_11.tar.gz
RUN cd doxygen-Release_1_8_11/ && mkdir build \
&& cd build \
&& cmake -G "Unix Makefiles" .. \
&& make \
&& make install

RUN pip3 install scikit-build \
	&& pip3 install meson ninja \
	&& export PATH="$PATH:~/.local/bin"

RUN git clone https://gitlab.freedesktop.org/libnice/libnice \
    && cd libnice \
    && meson --prefix=/usr build && ninja -C build && ninja -C build install

RUN cd ~ \
    && git clone https://github.com/freeswitch/sofia-sip.git \
    && cd sofia-sip \
    && sh autogen.sh \
    && ./configure \
    && make \
    && make install

RUN cd ~ \
    && git clone https://github.com/cisco/libsrtp.git \
    && cd libsrtp \
    && ./configure --prefix=/usr --enable-openssl \
    && make shared_library \
    && make install

RUN cd ~ \
    && git clone https://github.com/sctplab/usrsctp \
    && cd usrsctp \
    && ./bootstrap \
    && ./configure --prefix=/usr \
    && make \
    && make install

RUN cd ~ \
    && git clone https://github.com/warmcat/libwebsockets.git \
    && cd libwebsockets \
    && mkdir build \
    && cd build \
    && cmake -DLWS_MAX_SMP=1 -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_C_FLAGS="-fpic" .. \
    && make \
    && make install
RUN cd ~ \ 
	&& git clone https://github.com/eclipse/paho.mqtt.c.git \
	&& cd paho.mqtt.c \
	&& make && make install 

RUN cd ~ \
	&& git clone https://github.com/alanxz/rabbitmq-c \
	&& cd rabbitmq-c \
	&& git submodule init \
	&& git submodule update \
	&& mkdir build && cd build \
	&& cmake -DCMAKE_INSTALL_PREFIX=/usr .. \
	&& make && make install
RUN apt install curl gcc g++ make -y    
RUN curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -
RUN apt install -y nodejs
RUN cd /tmp
    && git clone https://github.com/meetecho/janus-gateway.git \
    && cd janus-gateway \
    && sh autogen.sh \
    && ./configure --prefix=/opt/janus-tools --enable-post-processing  \
    && make CFLAGS='-std=c99' \
    && make install 
ENV PATH "$PATH:/opt/janus-tools/bin"