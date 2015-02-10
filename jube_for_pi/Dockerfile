FROM resin/rpi-raspbian
MAINTAINER Steve Fisher <stevef1uk@gmail.com> (@stevef)

ADD jdk-8u33-linux-arm-vfp-hflt.tar.gz /jdk-8u33-linux-arm-vfp-hflt.tar.gz

RUN apt-get update && DEBIAN_FRONTEND=noninteractive && apt-get install -y \
    curl \
    zip 

#RUN tar zxvf /jdk-8u33-linux-arm-vfp-hflt.tar.gz -C /opt
RUN ln -s /jdk-8u33-linux-arm-vfp-hflt.tar.gz/jdk1.8.0_33/ /opt/jdk1.8.0
RUN update-alternatives --install /usr/bin/javac javac /opt/jdk1.8.0/bin/javac 1
RUN update-alternatives --install /usr/bin/java java /opt/jdk1.8.0/bin/java 1
RUN  update-alternatives --config javac
RUN update-alternatives --config java

RUN curl -O http://central.maven.org/maven2/io/fabric8/jube/images/jube/jube/2.0.25/jube-2.0.25-image.zip

RUN mkdir jube-2.0.25-image; cd jube-2.0.25-image; unzip ../jube-2.0.25-image.zip;\
    rm ../jube-2.0.25-image.zip; chmod +x *.sh *.bat bin/*

ENV KUBERNETES_MASTER=http://localhost:8585/
ENV FABRIC8_CONSOLE=http://localhost:8585/hawtio/

CMD jube-2.0.25-image/start.sh && tail -f jube-2.0.25-image/logs/err.log
