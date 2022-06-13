FROM ubuntu:18.04
RUN apt-get update -y 
RUN apt-get install nginx -y
RUN apt-get install curl -y 

RUN rm -rf /var/www/html/index.nginx-debian.html 
RUN mkdir -p  /var/www/html/img/
COPY /img/pp.png  /var/www/html/img/
COPY index.html script.js style.css /img   /var/www/html/

EXPOSE 80/tcp 
CMD ["/usr/sbin/nginx", "-g", "daemon off;"] 



