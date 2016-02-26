FROM ubuntu:12.04

MAINTAINER Zukin

RUN apt-get update
RUN apt-get install -y nginx
RUN rm /etc/nginx/sites-available/default
ADD default /etc/nginx/sites-available/default
RUN mkdir /root/scripts
ADD start.sh /root/scripts/start.sh
RUN chmod 777 /root/scripts/start.sh
RUN echo "127.0.1.1 em.opstree.com" >> /etc/hosts
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
EXPOSE 80
#ENTRYPOINT ["sh", "/root/scripts/start.sh"]
CMD service nginx start
