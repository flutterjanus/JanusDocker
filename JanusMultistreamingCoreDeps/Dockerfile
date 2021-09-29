FROM shivanshtalwar0/januscoredeps

LABEL maintainer="shivansh talwar <shivanshtalwar0@gmail.com>"
LABEL description="Provides an image with Janus Gateway"


RUN echo "reload gd"	    
RUN cd ~ \
    && git clone https://github.com/meetecho/janus-gateway.git \
    && cd janus-gateway \
    && git checkout multistream \
    && git pull \
    && sh autogen.sh \
    && ./configure --prefix=/opt/janus --enable-docs \
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
#CMD /bin/sh 
CMD /opt/janus/bin/janus
