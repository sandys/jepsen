FROM bitnami/minideb:stretch
MAINTAINER sss@lambdacurry.com

ENV LEIN_ROOT true
ENV DEBIAN_FRONTEND noninteractive

#
# Jepsen dependencies
#
RUN apt-get -y -q update && \
    apt-get -y -q install software-properties-common && \
    apt-get -y -q update && \
    apt-get install -qy \
        libjna-java \
        git \
        wget \
	vim # not required by jepsen itself, just for ease of use

RUN mkdir /opt/jre &&\
    wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" -O /opt/jre/jre.tar.gz http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/server-jre-8u131-linux-x64.tar.gz &&\
    tar xpfo /opt/jre/jre.tar.gz -C /opt/jre --strip 1 &&\
    rm /opt/jre/jre.tar.gz

ENV JAVA_HOME /opt/jre
ENV PATH $PATH:$JAVA_HOME/bin

RUN wget https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein && \
    mv lein /usr/bin && \
    chmod +x /usr/bin/lein && \
    lein self-install

# You need to locate jepsen in this directory (up.sh does that automatically)
ADD jepsen /jepsen
RUN cd /jepsen/jepsen && lein install

ADD ./bashrc /root/.bashrc
ADD ./init.sh /init.sh
RUN chmod +x /init.sh

RUN mkdir -p /workdir
WORKDIR /workdir

#sudo docker run --rm -it  -v "$PWD":/workdir -w /workdir jepsen-control  lein new sss

CMD /init.sh
