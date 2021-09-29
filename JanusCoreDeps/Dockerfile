FROM ubuntu:bionic

LABEL maintainer="shivansh talwar <shivanshtalwar0@gmail.com>"
LABEL description="Provides an image with Janus Gateway"

RUN apt-get update -y \
    && apt-get upgrade -y \
    && apt install aptitude wget -y
        
RUN aptitude install -y libmicrohttpd-dev libjansson-dev \
	libssl-dev libsrtp-dev libsofia-sip-ua-dev libglib2.0-dev \
	libopus-dev libogg-dev libcurl4-openssl-dev liblua5.3-dev \
	libconfig-dev pkg-config gengetopt libtool automake libnanomsg-dev graphviz

RUN apt-get install -y \
    sudo \
    make \
    git \
    cmake \
    graphviz

#RUN apt purge cmake -y
    

RUN wget https://github.com/doxygen/doxygen/archive/refs/tags/Release_1_8_11.tar.gz
RUN tar -xf Release_1_8_11.tar.gz
RUN apt-get install flex bison -y
RUN cd doxygen-Release_1_8_11/ && mkdir build \
&& cd build \
&& cmake -G "Unix Makefiles" .. \
&& make \
&& make install

RUN apt-get install python3 python3-pip nginx -y \
	&& pip3 install meson ninja \
	&& export PATH="$PATH:~/.local/bin"

RUN echo $PATH    
RUN git clone https://gitlab.freedesktop.org/libnice/libnice \
    && cd libnice \
    && meson --prefix=/usr build && ninja -C build && ninja -C build install
    
    
    
RUN cd ~ \
    && git clone https://github.com/cisco/libsrtp.git \
    && cd libsrtp \
    && ./configure --prefix=/usr --enable-openssl \
    && make shared_library \
    && make install
# 
RUN cd ~ \
    && git clone https://github.com/sctplab/usrsctp \
    && cd usrsctp \
    && ./bootstrap \
    && ./configure --prefix=/usr \
    && make \
    && make install
# 
RUN cd ~ \
    && git clone https://github.com/warmcat/libwebsockets.git \
    && cd libwebsockets \
    && mkdir build \
    && cd build \
    && cmake -DLWS_MAX_SMP=1 -DCMAKE_INSTALL_PREFIX:PATH=/usr -DCMAKE_C_FLAGS="-fpic" .. \
    && make \
    && make install
Run cd ~ \ 
	&& git clone https://github.com/eclipse/paho.mqtt.c.git \
	&& cd paho.mqtt.c \
	&& make && make install 


RUN wget https://github.com/Kitware/CMake/releases/download/v3.20.1/cmake-3.20.1.tar.gz
RUN tar -xf cmake-3.20.1.tar.gz
RUN cd cmake-3.20.1/ && ./bootstrap && make -j$(nproc) && make install
RUN echo "cmake installed!"

RUN cd ~ \
	&& git clone https://github.com/alanxz/rabbitmq-c \
	&& cd rabbitmq-c \
	&& git submodule init \
	&& git submodule update \
	&& mkdir build && cd build \
	&& cmake -DCMAKE_INSTALL_PREFIX=/usr .. \
	&& make && make install

RUN echo "reload gd"	    
 
EXPOSE 7088 8088 8188 8089 7188
EXPOSE 10000-10200/udp
