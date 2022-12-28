FROM ubuntu:18.04
LABEL maintainer = "Rick Yang <my931uow@gmail.com>"
RUN apt-get update \
    && apt-get install -y apache2 
RUN echo "TEST WEB PAGE" > /var/www/html/index.html
EXPOSE 80
CMD ["/usr/sbin/apache2ctl", "-DFOREGROUND"]

