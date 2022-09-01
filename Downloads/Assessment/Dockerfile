FROM ubuntu:16.04
RUN apt-get update -y
RUN apt-get install nginx -y
COPY index.html /var/www/html
RUN service nginx start
EXPOSE 80