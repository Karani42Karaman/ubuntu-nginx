########################################################################################
# Beispiel 4
#
# Erstellen Sie ein Dockerfile, welches Ubuntu als Quelle nutzt.
# Installieren Sie nginx mittels source code.
# Nutzen Sie den Mutlistage build.
# Erstellen und starten Sie den docker container.
#
# Image bauen: docker build -t christianheimke/nginx:nginx_multistage .
# Image ausführen: docker run -p 8080:80 christianheimke/nginx:nginx_multistage
########################################################################################

# Basis-Image festlegen mit einem Alias
# https://docs.docker.com/engine/reference/builder/#from
########################################################################################
FROM ubuntu:18.04 as builder

# setzen von Konfigurations-Umgebungsvariablen als Mehrzeiler für mehr Übersichtlichkeit
# verhindert das der build-Prozess auf die Eingabe bei der Landesauswahl stehen bleibt
# siehe https://rtfm.co.ua/en/docker-configure-tzdata-and-timezone-during-build/
# https://docs.docker.com/engine/reference/builder/#env
########################################################################################
ENV TZ=Europe/Berlin \
    DEBIAN_FRONTEND=noninteractive

# hinzufügen des Archivs, es wird direkt entpackt
# https://docs.docker.com/engine/reference/builder/#add
########################################################################################
ADD nginx-1.20.0.tar.gz /usr/src/nginx

# festlegen es WORKDIR um die konfiguration und Installation zu vereinfachen
# https://docs.docker.com/engine/reference/builder/#workdir
########################################################################################
WORKDIR /usr/src/nginx/nginx-1.20.0

# Paketmanager aktualisieren und abhängigkeiten installieren
# https://www.alibabacloud.com/blog/how-to-build-nginx-from-source-on-ubuntu-20-04-lts_597793
########################################################################################
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

# source code konfigurieren, kompilieren (make) und Ergebnis installieren (make install)
# https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#compiling-and-installing-from-source
########################################################################################
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
# https://docs.docker.com/engine/reference/builder/#from
########################################################################################
FROM ubuntu:18.04

# Portfreigabe
# https://docs.docker.com/engine/reference/builder/#expose
########################################################################################
EXPOSE 80

# festlegen des WORKDIR des document roots
# https://docs.docker.com/engine/reference/builder/#workdir
########################################################################################
#WORKDIR /var/www/html

# executable Datei des builder-stage kopieren
# https://docs.docker.com/engine/reference/builder/#copy
########################################################################################
COPY --from=builder /usr/sbin/nginx /usr/sbin/

# Konfigurationsdateien des builder-stage kopieren
# https://docs.docker.com/engine/reference/builder/#copy
########################################################################################
COPY --from=builder /etc/nginx/ /etc/nginx/

COPY --from=builder /var/www/html/ /var/www/html/
#application
RUN mkdir -p  /var/www/html/html/img/
COPY /img/pp.png  /var/www/html/html/img/
COPY index.html script.js style.css    /var/www/html/html/


# log-Verzeichnis anlegen
########################################################################################
RUN mkdir -p /var/log/nginx/

# libssl installieren
########################################################################################
RUN apt update && apt-get install -y libssl-dev

# temporäre Dateien löschen
########################################################################################
RUN apt autoclean && apt autoremove && rm -rf /var/lib/{apt,dpkg,cache,log}








# Startkommando mit Parametern
# https://docs.docker.com/engine/reference/builder/#cmd
########################################################################################
CMD ["nginx", "-g", "daemon off;"]
