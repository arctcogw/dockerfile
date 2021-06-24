FROM php:7.4-fpm
RUN cd /usr/bin && curl -s http://getcomposer.org/installer | php \
    && ln -s /usr/bin/composer.phar /usr/bin/composer
RUN apt-get update \
    && apt-get install -y \
    git \
    zip \
    unzip \
    vim
RUN apt-get update \
    && docker-php-ext-install pdo_mysql
RUN composer global require "laravel/installer"
EXPOSE 80
WORKDIR /var/www
RUN git clone https://github.com/arctcogw/laravel-tasks.git
WORKDIR /var/www/laravel-tasks
RUN composer install
COPY .env ./.env
COPY startup.sh /startup.sh
RUN chmod 744 /startup.sh
CMD ["/startup.sh"]
