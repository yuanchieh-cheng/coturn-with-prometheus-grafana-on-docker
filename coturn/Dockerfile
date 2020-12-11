FROM ubuntu:20.04

RUN apt-get update && apt-get upgrade
RUN export DEBIAN_FRONTEND=noninteractive && apt-get install -y curl unzip build-essential git debhelper dpkg-dev libssl-dev libevent-dev libmongoc-1.0-0 libbson-1.0-0
RUN export DEBIAN_FRONTEND=noninteractive && \
	apt-get update && \
	apt-get install -y build-essential git debhelper dpkg-dev libssl-dev libevent-dev libmongoc-dev libbson-dev
RUN export DEBIAN_FRONTEND=noninteractive && apt-get install -y libc6

RUN curl -fL -o /tmp/libprom.deb \
         https://github.com/digitalocean/prometheus-client-c/releases/download/v0.1.1/libprom-dev-0.1.1-Linux.deb

RUN curl -fL -o /tmp/libpromhttp.deb \
         https://github.com/digitalocean/prometheus-client-c/releases/download/v0.1.1/libpromhttp-dev-0.1.1-Linux.deb


RUN apt install -y /tmp/libprom.deb
RUN apt install -y /tmp/libpromhttp.deb


RUN curl https://codeload.github.com/coturn/coturn/zip/master --output master.zip
RUN unzip master.zip
RUN cd coturn-master && ./configure && make

COPY ./turnserver.conf /etc/turnserver.conf
COPY ./my-alfred.com.certs.pem /usr/local/etc/my-alfred.com.certs.pem
COPY ./my-alfred.com.priv.pem /usr/local/etc/my-alfred.com.priv.pem

EXPOSE 3478/udp
EXPOSE 3478/tcp
EXPOSE 9641/tcp
EXPOSE 49152-65535

CMD coturn-master/bin/turnserver -v -c /etc/turnserver.conf --static-auth-secret=${static-auth-secret}