# Standard Operating Environment running off Ubuntu 14.04 & PHP 5.6.

FROM chinthakagodawita/base:ubuntu-14.04
MAINTAINER Chinthaka Godawita <chin.godawita@me.com>

# Install all packages.
RUN apt-get update

## Apache.
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install apache2

## Memcached.
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install memcached

## MySQL client.
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install mysql-client

## postfix.
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install postfix


# Configure Apache.
COPY ./conf/apache2/httpd-vhosts.conf /etc/apache2/sites-available/000-default.conf
RUN a2enmod vhost_alias
RUN a2enmod rewrite
RUN a2enmod headers



# PHP.
RUN add-apt-repository ppa:ondrej/php5-5.6
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install \
  php5 \
  php5-gd \
  php5-dev \
  php5-curl \
  php5-mcrypt \
  php5-mysql \
  php5-memcached \
  php-soap \
  php-pear

# Configure PHP.
ADD ./conf/php/30-overrides.ini /etc/php5/apache2/conf.d/30-overrides.ini

# Enable PHP modules.
RUN php5enmod opcache

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install drush.
RUN composer global require drush/drush:7.*




# Startup settings.
ADD ./conf/supervisor/php.conf /etc/supervisor/conf.d/php.conf

# HTTP.
EXPOSE 80

WORKDIR /var/www

CMD ["/usr/bin/supervisord", "-n"]
