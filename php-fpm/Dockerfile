FROM php:5.6-fpm
MAINTAINER Michel Silva <michel_silva_04@hotmail.com>

ENV MEMORY_LIMIT 512M

#PDO_MYSQL
RUN docker-php-ext-install pdo pdo_mysql mysqli mbstring

#INSTALL PHP-MONGO / APC
RUN apt-get update
RUN apt-get install -y g++ libmcrypt-dev php5-dev php5-fpm libxml2-dev
RUN pecl install apcu-4.0.11 \
    && echo extension=apcu.so > /usr/local/etc/php/conf.d/apcu.ini
RUN apt-get update \
	&& apt-get -y install libssl-dev

#MONGO
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
	&& docker-php-ext-enable mongo

#XDEBUG
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug 
RUN sed -i '1 a xdebug.remote_autostart=true' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_mode=req' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_handler=dbgp' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_connect_back=1 ' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_port=9000' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_host=127.0.0.1' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -i '1 a xdebug.remote_enable=1' /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN sed -ri 's/^max_execution_time\s*=\s*.+/max_execution_time = 3000/g' /etc/php5/fpm/php.ini
RUN sed -i -e "s/;display_errors = Off/display_errors = On/g" /etc/php5/fpm/php.ini
RUN sed -i "s@^memory_limit =.*@memory_limit = $MEMORY_LIMIT@" /etc/php5/fpm/php.ini


#MCRYPT
RUN docker-php-ext-install mcrypt \
    && docker-php-ext-enable mcrypt

RUN docker-php-ext-install soap
RUN apt-get install php5-xmlrpc wget -y

RUN sed -i -e "s/;error_log = php_errors.log/error_log = php_errors.log/g" /etc/php5/fpm/php.ini
RUN sed -i -e "s/error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT/error_reporting = E_ALL/g" /etc/php5/fpm/php.ini
RUN sed -i -e "s/;log_level = notice/log_level = debug/g" /etc/php5/fpm/php-fpm.conf
RUN apt-get -y install vim

RUN sed -i -e "s/;listen.mode = 0660/listen.mode = 0660/g" /etc/php5/fpm/pool.d/www.conf
