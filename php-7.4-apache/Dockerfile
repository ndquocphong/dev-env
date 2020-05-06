FROM php:7.4-apache

# Download script to install PHP extensions and dependencies
ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/

RUN chmod uga+x /usr/local/bin/install-php-extensions && sync

RUN DEBIAN_FRONTEND=noninteractive apt-get update -q \
    && DEBIAN_FRONTEND=noninteractive apt-get install -qq -y \
      curl \
      git \
      zip unzip \
      vim \
      openssh-server \
    && install-php-extensions \
      bcmath \
      bz2 \
      calendar \
      exif \
      gd \
      intl \
      ldap \
      memcached \
      mysqli \
      opcache \
      pdo_mysql \
      pdo_pgsql \
      pgsql \
      redis \
      soap \
      xsl \
      zip \
      sockets \
      xdebug \
# already installed:
#      iconv \
#      mbstring \
    && a2enmod rewrite

RUN touch /usr/local/etc/php/conf.d/custom.ini    

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php -- --filename=composer --install-dir=/usr/local/bin
RUN chown -R www-data: /var/www
USER www-data
RUN composer global require --optimize-autoloader "hirak/prestissimo"

# Config apache2
USER root    
RUN echo "ServerName localhost" | tee /etc/apache2/conf-available/fqdn.conf
RUN mkdir -p /etc/apache2/vhosts
RUN echo "IncludeOptional vhosts/*.conf" >> /etc/apache2/apache2.conf
RUN a2enconf fqdn
RUN a2enmod rewrite
RUN a2enmod deflate 
RUN service apache2 restart