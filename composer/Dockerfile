FROM composer:latest

###########################################################################
# HOST MACHINE USER
###########################################################################
ARG PHPGROUP
ARG PHPUSER

ENV PHPGROUP=${PHPGROUP}
ENV PHPUSER=${PHPUSER}

RUN adduser -g ${PHPGROUP} -s /bin/sh -D ${PHPUSER}; exit 0

WORKDIR /usr/local/apache2/htdocs