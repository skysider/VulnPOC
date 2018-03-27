FROM phusion/baseimage:latest
LABEL maintainer=skysider@163.com

RUN apt-get -y update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    wget \
    xz-utils \
    make \
    gcc \
    libpcre++-dev \
    libdb-dev \
    libxt-dev \
    libxaw7-dev \
    tzdata \
    telnet && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /work 

COPY conf.conf /work/

RUN wget https://github.com/Exim/exim/releases/download/exim-4_89/exim-4.89.tar.xz && \
    tar xf exim-4.89.tar.xz && cd exim-4.89 && \
    cp src/EDITME Local/Makefile && cp exim_monitor/EDITME Local/eximon.conf && \
    sed -i 's/# AUTH_CRAM_MD5=yes/AUTH_CRAM_MD5=yes/' Local/Makefile && \
    sed -i 's/^EXIM_USER=/EXIM_USER=exim/' Local/Makefile && \
    useradd exim && make && mkdir -p /var/spool/exim/log && \
    cd /var/spool/exim/log && touch mainlog paniclog rejectlog && \
    chown exim mainlog paniclog rejectlog && \
    echo "Asia/Shanghai" > /etc/timezone && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

CMD ["/work/exim-4.89/build-Linux-x86_64/exim", "-bd", "-d-receive", "-C", "conf.conf"]