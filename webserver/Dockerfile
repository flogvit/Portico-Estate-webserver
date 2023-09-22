#
# Apache/php-server with xsl, postgres, mysql, mssql
#

FROM php:8.2-apache
LABEL maintainer="Sigurd Nes <sigurdne@online.no>"
RUN apt update

# php8.2
RUN apt-get install -y libaio1 locales libcurl4-openssl-dev libicu-dev libpq-dev zlib1g-dev libpng-dev libxslt1-dev
RUN docker-php-ext-install mysqli pdo pdo_mysql curl intl pgsql gd xsl imap soap zip mbstring && a2enmod rewrite
RUN apt-get install -y apg

# REDIS (php 8.2)
# https://askubuntu.com/questions/1433463/php8-1-redis-module-for-ubuntu-20-04

RUN locale-gen --purge en_US.UTF-8
ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# Supervisor #
RUN mkdir -p /var/log/supervisor
ADD supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Apache2
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APP_DOCUMENT_ROOT /var/www/html


EXPOSE 80

RUN a2enmod rewrite
RUN a2enmod headers


# PHP
ENV PHP_INI ""
ENV XDEBUG_REMOTE_PORT ""
#
ADD php8/app.ini /etc/php/8.2/apache2/conf.d/91-app.ini
ADD php8/app.ini /etc/php/8.2/cli/conf.d/91-app.ini


# Add image configuration and scripts
ADD start.sh /start.sh
ADD run.sh /run.sh
RUN chmod 755 /*.sh


RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Others
ENV HOME /root
WORKDIR /root


# Command on startup
CMD ["/run.sh"]


# NOTE TO SELF:
# docker build -t sigurdne/portico_estate_webserver .
# docker run --name sigurdne_portico_estate_webserver -i -t sigurdne/portico_estate_webserver
# docker exec -i -t sigurdne_portico_estate_webserver bash #by Name
# docker rm $(docker ps -a -q)
# docker push sigurdne/portico_estate_webserver
