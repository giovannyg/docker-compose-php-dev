FROM composer:2.1

###########################################################################
# HOST MACHINE USER
###########################################################################
ARG PHPGROUP
ARG PHPUSER

ENV PHPGROUP=${PHPGROUP}
ENV PHPUSER=${PHPUSER}

RUN adduser -g ${PHPGROUP} -s /bin/sh -D ${PHPUSER}; exit 0

RUN composer global require laravel/installer

ENV PATH=/tmp/vendor/bin:$PATH

WORKDIR /usr/local/apache2/htdocs


