FROM toixtran/ubuntu:16.04

MAINTAINER toi.xtran <toi.xtran@gmail.com>

# Update & upgrade
RUN apt-get update && \
    apt-get -y upgrade

# Install common packages
RUN apt-get -y install \
    apache2 \
    php \
    php-cli \
    php-gd \
    php-json \
    php-mbstring \
    php-xml \
    php-xsl \
    php-zip \
    php-soap \
    php-pear \
    php-mcrypt \
    libapache2-mod-php \
    php-curl \
    php-mysql \
    php-dev \
    php-xdebug

# Enable mode
RUN a2enmod rewrite
RUN phpenmod mcrypt

# Composer install
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

# Apache2 config
RUN mkdir -p /var/lock/apache2 /var/run/apache2
ADD /root/etc/apache2/sites-enabled/000-default.conf /etc/apache2/sites-enabled/000-default.conf
ADD /root/etc/apache2/apache2.conf /etc/apache2/apache2.conf
ADD /root/etc/apache2/envvars /etc/apache2/envvars

# Update php.ini
ADD /root/etc/php/7.0/apache2/php.ini /etc/php/7.0/apache2/php.ini

# Chown own public dir to www-data
ADD /root/var/www/html/public/index.php /var/www/html/public/index.php
RUN chown -R www-data:www-data /var/www/html/public

# Run apache when container start using supervisord
ADD /root/etc/supervisord.d/apache2.ini /etc/supervisord.d/apache2.ini

# Xdebug config
ENV XDEBUGINI_PATH=/etc/php/7.0/mods-available/xdebug.ini
RUN echo "xdebug.idekey = PHPSTORM" >> $XDEBUGINI_PATH && \
    echo "xdebug.default_enable = 0" >> $XDEBUGINI_PATH && \
    echo "xdebug.remote_enable = 1" >> $XDEBUGINI_PATH && \
    echo "xdebug.remote_autostart = 0" >> $XDEBUGINI_PATH && \
    echo "xdebug.remote_connect_back = 0" >> $XDEBUGINI_PATH && \
    echo "xdebug.profiler_enable = 0" >> $XDEBUGINI_PATH && \
    echo "xdebug.remote_host="`/sbin/ip route|awk '/default/ { print $3 }'` >> $XDEBUGINI_PATH

# Clean
RUN rm -rf /var/lib/apt/lists/*

EXPOSE 80

CMD ["/usr/bin/supervisord", "--configuration=/etc/supervisord.conf"]