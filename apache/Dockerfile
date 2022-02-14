FROM httpd:2.4.35-alpine
RUN apk update; \
    apk upgrade; 
COPY ./apache.conf /usr/local/apache2/conf/httpd.conf
COPY ./httpd-vhosts.conf /usr/local/apache2/conf/extra/httpd-vhosts.conf
EXPOSE 80
#EXPOSE 443

RUN apk add --no-cache tzdata
ENV TZ America/Mexico_City