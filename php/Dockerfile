###########################################################################
# Debian OS 10
###########################################################################
FROM php:7.4-fpm

###########################################################################
# root empty pass
###########################################################################
RUN passwd -d root

###########################################################################
# HOST MACHINE USER
###########################################################################
ARG PHPUSER=giovanny
RUN groupadd -g 1000 ${PHPUSER} \
    && useradd -m -s /bin/bash -u 1000 -g 1000 ${PHPUSER}

###########################################################################
# OS libraries
###########################################################################
RUN apt-get update && apt-get install -y \
  wget \
  apt-utils \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libpng-dev \
  libzip-dev \
  zip \
  libxml2-dev \
  libxslt1-dev \
  lsof \
  default-mysql-client \
  gnupg2 \
  apt-transport-https \
  procps \
  iputils-ping \
  libpq-dev

###########################################################################
# Custom php.ini
COPY php.ini /usr/local/etc/php/php.ini
###########################################################################

###########################################################################
# PHP source extensions
###########################################################################
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
  && docker-php-ext-install -j "$(nproc)" gd \
  && docker-php-ext-install zip soap mysqli pdo_mysql pdo_pgsql \
  bcmath intl xsl sockets

###########################################################################
# PECL extensions
###########################################################################
ARG INSTALL_XDEBUG=true

RUN if [ ${INSTALL_XDEBUG} = true ]; then \
  pecl install xdebug \
  && docker-php-ext-enable xdebug \
;fi

COPY docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

ENV XDEBUG_SESSION=1
###########################################################################
# Elastic search
###########################################################################
ARG INSTALL_ELASTIC_SEARCH=false
ENV PATH=/usr/share/elasticsearch/bin:$PATH
RUN if [ ${INSTALL_ELASTIC_SEARCH} = true ]; then \
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | apt-key add - \
  && echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | tee /etc/apt/sources.list.d/elastic-7.x.list \
  && apt-get update && apt-get install -y elasticsearch \
  && update-rc.d elasticsearch defaults 95 10 \
  && service elasticsearch start \
;fi

###########################################################################
# Microsoft ODBC Driver & PDO_SQLSRV
###########################################################################
ARG INSTALL_PDO_SQLSRV=false
ENV PATH=/opt/mssql-tools/bin:$PATH
RUN if [ ${INSTALL_PDO_SQLSRV} = true ]; then \
apt-get update && apt-get install -y gnupg2 \
  && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
  && curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list \
  && apt-get update \
  && ACCEPT_EULA=Y apt-get install -y msodbcsql17 \
  && ACCEPT_EULA=Y apt-get install -y mssql-tools \
  && apt-get install -y unixodbc-dev \
  && apt-get install -y libgssapi-krb5-2 \
  && apt-get update \ 
  && apt-get -y install unixodbc-dev \
  && pecl install sqlsrv pdo_sqlsrv \
  && echo "\nextension=pdo_sqlsrv.so\nextension=sqlsrv.so" >> /usr/local/etc/php/php.ini \
;fi

###########################################################################
# ODBC Driver for Oracle & PHP_OCI8
###########################################################################

#WORKDIR /usr/local/apache2/htdocs

#RUN apt-get update && apt-get -y install wget bsdtar libaio1 && \
  #wget -qO- https://raw.githubusercontent.com/caffeinalab/php-fpm-oci8/master/oracle/instantclient-basic-linux.x64-12.2.0.1.0.zip | bsdtar -xvf- -C /usr/local && \
  #wget -qO- https://raw.githubusercontent.com/caffeinalab/php-fpm-oci8/master/oracle/instantclient-sdk-linux.x64-12.2.0.1.0.zip | bsdtar -xvf-  -C /usr/local && \
  #wget -qO- https://raw.githubusercontent.com/caffeinalab/php-fpm-oci8/master/oracle/instantclient-sqlplus-linux.x64-12.2.0.1.0.zip | bsdtar -xvf- -C /usr/local && \
  #ln -s /usr/local/instantclient_12_2 /usr/local/instantclient && \
  #ln -s /usr/local/instantclient/libclntsh.so.* /usr/local/instantclient/libclntsh.so && \
  #ln -s /usr/local/instantclient/lib* /usr/lib && \
  #ln -s /usr/local/instantclient/sqlplus /usr/bin/sqlplus && \
  #docker-php-ext-configure oci8 --with-oci8=instantclient,/usr/local/instantclient && \
  #docker-php-ext-install oci8 && \
  #rm -rf /var/lib/apt/lists/* && \
  #php -v

#RUN wget http://php.net/distributions/php-7.4.0.tar.gz && \
  #mkdir php_oci && \
  #mv php-7.4.0.tar.gz ./php_oci

#WORKDIR /usr/local/apache2/htdocs/php_oci
#RUN tar xfvz php-7.4.0.tar.gz

#WORKDIR /usr/local/apache2/htdocs/php_oci/php-7.4.0/ext/pdo_oci
#RUN phpize && \
  #./configure --with-pdo-oci=instantclient,/usr/local/instantclient,12.1 && \
  #make && \
  #make install && \
  #echo extension=pdo_oci.so > /usr/local/etc/php/conf.d/pdo_oci.ini && \
  #php -v

###########################################################################
# Install Composer
###########################################################################
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
&& composer global require laravel/installer

ENV PATH=/root/.composer/vendor/bin:$PATH

###########################################################################
# Linux Alias
###########################################################################
RUN echo "alias l='ls -lah'" >> /home/${PHPUSER}/.bashrc >> /root/.bashrc \
    && echo "alias -- -='cd -'" >> /home/${PHPUSER}/.bashrc >> /root/.bashrc \
    && echo "alias -- ..='cd ..'" >> /home/${PHPUSER}/.bashrc >> /root/.bashrc

WORKDIR /usr/local/apache2/htdocs


