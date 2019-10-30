FROM php:7.2-apache

RUN apt-get update \
  && apt-get install -y \
  zlib1g-dev libxml2-dev libzip-dev libicu-dev g++ zip libpng-dev \
  && docker-php-ext-configure zip --with-libzip \
  && docker-php-ext-install mysqli \
  && docker-php-ext-install mbstring \
  && docker-php-ext-install zip \
  && docker-php-ext-configure intl \
  && docker-php-ext-install intl \
  && docker-php-ext-install gd \
  && docker-php-ext-install soap \
  && docker-php-ext-install xmlrpc \
  && docker-php-ext-install opcache

COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

ENV APACHE_DOCUMENT_ROOT /var/moodle

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

WORKDIR /var

RUN curl https://download.moodle.org/download.php/direct/stable37/moodle-latest-37.tgz --output moodle.tgz
RUN tar zxvf moodle.tgz
RUN rm moodle.tgz
RUN mkdir /var/moodledata
RUN chown -R 'www-data':'www-data' /var/moodledata
RUN chmod -R 700 /var/moodledata
ADD ./config.php /var/moodle

WORKDIR /var/moodle
