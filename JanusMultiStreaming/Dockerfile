FROM ubuntu:18.04

LABEL maintainer="shivansh talwar <shivanshtalwar0@gmail.com>"
LABEL description="Provides an image with Janus Gateway"

RUN apt-get update -y \
    && apt-get upgrade -y \
    && apt install aptitude -y
        
RUN aptitude install libmicrohttpd-dev libjansson-dev \
	libssl-dev libsrtp-dev libsofia-sip-ua-dev libglib2.0-dev \
	libopus-dev libogg-dev libcurl4-openssl-dev liblua5.3-dev \
	libconfig-dev pkg-config gengetopt libtool automake libnanomsg-dev doxygen graphviz -y

RUN apt-get install -y \
    sudo \
    make \
    git \
    doxygen \
    graphviz \
    cmake
RUN apt install python3 python3-pip nginx -y \
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

RUN cd ~ \
	&& git clone https://github.com/alanxz/rabbitmq-c \
	&& cd rabbitmq-c \
	&& git submodule init \
	&& git submodule update \
	&& mkdir build && cd build \
	&& cmake -DCMAKE_INSTALL_PREFIX=/usr .. \
	&& make && make install
	    
RUN cd ~ \
    && git clone https://github.com/meetecho/janus-gateway.git \
    && cd janus-gateway \
    && git checkout multistream \
    && sh autogen.sh \
    && ./configure --prefix=/opt/janus \
    && make CFLAGS='-std=c99' \
    && make install \
    && make configs
# 
# RUN cp -rp ~/janus-gateway/certs /opt/janus/share/janus
# 
COPY conf/*.jcfg /opt/janus/etc/janus/
# 
# RUN apt-get install nginx -y
# COPY nginx/nginx.conf /etc/nginx/nginx.conf
# 
EXPOSE 7088 8088 8188 8089 7188
EXPOSE 10000-10200/udp
# 
CMD /opt/janus/bin/janus
