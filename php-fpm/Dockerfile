FROM php:5.6-fpm
MAINTAINER Michel Silva <michel_silva_04@hotmail.com>

#PDO_MYSQL
RUN docker-php-ext-install pdo pdo_mysql

#INSTALL PHP-MONGO / APC
RUN apt-get update
RUN apt-get install -y g++ php5-dev 
RUN pecl install apcu-4.0.11 \
    && echo extension=apcu.so > /usr/local/etc/php/conf.d/apcu.ini
RUN apt-get update \
	&& apt-get -y install libssl-dev
COPY mongo-php-driver-legacy-1.6.14.tar.gz /tmp/mongo.tar.gz
RUN tar xfz /tmp/mongo.tar.gz -C /tmp/ \
	&& mkdir -p /usr/src/php/ext/mongo \
	&& mv /tmp/mongo-php-driver-legacy-1.6.14/* /usr/src/php/ext/mongo \
	&& rm -r /tmp/mongo.tar.gz /tmp/mongo-php-driver-legacy-1.6.14 \
	&& cd /usr/src/php/ext/mongo \
	&& phpize \
	&& ./configure \
	&& make all \
	&& docker-php-ext-install mongo\
	&& docker-php-ext-enable mongo\
