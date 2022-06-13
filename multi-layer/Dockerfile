FROM ubuntu:18.04 as builder
ENV TZ=Europe/Berlin \
    DEBIAN_FRONTEND=noninteractive
ADD nginx-1.20.0.tar.gz /usr/src/nginx
WORKDIR /usr/src/nginx/nginx-1.20.0

RUN apt update && apt-get install -y build-essential \
                                    libpcre3 \
                                    libpcre3-dev \
                                    zlib1g \
                                    zlib1g-dev \
                                    libssl-dev \
                                    libgd-dev \
                                    libxml2 \
                                    libxml2-dev \
                                    uuid-dev

RUN ./configure --prefix=/var/www/html \
                --sbin-path=/usr/sbin/nginx \
                --conf-path=/etc/nginx/nginx.conf \
                --http-log-path=/var/log/nginx/access.log \
                --error-log-path=/var/log/nginx/error.log \
                --with-pcre \
                --lock-path=/var/lock/nginx.lock \
                --pid-path=/var/run/nginx.pid \
                --with-http_ssl_module \
                --with-http_image_filter_module=dynamic \
                --modules-path=/etc/nginx/modules \
                --with-http_v2_module \
                --with-stream=dynamic \
                --with-http_addition_module \
                --with-http_mp4_module \
    && make && make install



########################################################################################
#           FINALE STAGE
########################################################################################

# Basis Image festlegen
FROM ubuntu:18.04

# Port
EXPOSE 80/tcp
COPY --from=builder /usr/sbin/nginx /usr/sbin/
# Configurations
COPY --from=builder /etc/nginx/ /etc/nginx/
COPY --from=builder /var/www/html/ /var/www/html/

#application
RUN mkdir -p  /var/www/html/html/img/
COPY /img/pp.png  /var/www/html/html/img/
COPY index.html script.js style.css    /var/www/html/html/

RUN mkdir -p /var/log/nginx/
RUN apt update && apt-get install -y libssl-dev
RUN apt autoclean && apt autoremove && rm -rf /var/lib/{apt,dpkg,cache,log}
CMD ["nginx", "-g", "daemon off;"]
