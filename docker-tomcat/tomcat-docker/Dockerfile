FROM ubuntu:12.04
MAINTAINER Zukin

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get -y upgrade
RUN apt-get install -y openssh-server python-setuptools && /usr/bin/easy_install supervisor

ADD adds/authorized_keys /authorized_keys
ADD adds/configure.sh /configure.sh
RUN bin/bash /configure.sh && rm /configure.sh

ADD adds/supervisord.conf /etc/supervisord.conf

EXPOSE 22

CMD ["/usr/local/bin/supervisord","-n"]

RUN apt-get update && \
    apt-get install -y openjdk-7-jdk && \
    cd /root && \
    apt-get install -y wget && \
    wget http://www.eu.apache.org/dist/tomcat/tomcat-7/v7.0.65/bin/apache-tomcat-7.0.65.tar.gz && \
    tar -xzvf apache-tomcat-7.0.65.tar.gz && \
    mv /root/apache-tomcat-7.0.65 /root/Zukin-tomcat

