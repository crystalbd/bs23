FROM php:8.2-fpm

WORKDIR /var/www/html
COPY ../src .

RUN apt-get update \
  && apt-get install -y build-essential zlib1g-dev default-mysql-client curl gn$
  && docker-php-ext-install zip pdo_mysql pdo_pgsql pgsql

RUN curl -sL https://deb.nodesource.com/setup_19.x | bash -
RUN apt-get install -y nodejs
RUN npm install npm@latest -g
RUN npm install yarn -g
RUN npm install -g @vue/cli

# Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin
RUN composer config --global process-timeout 3600
RUN composer global require "laravel/installer"
RUN composer setup

EXPOSE 8000

CMD ["php", "artisan", "serve", "--host", "0.0.0.0", "--port", "8000"]