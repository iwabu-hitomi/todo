FROM php:7.2-apache

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

ENV APACHE_DOCUMENT_ROOT /var/www/html/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN docker-php-ext-install sockets

RUN apt-get update && apt-get install -y \
    vim \
    git \
    unzip \
    zlib1g-dev \
    && docker-php-ext-install \
    zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
RUN curl -s http://getcomposer.org/installer | php && \
    echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer

RUN apt-get update && apt-get install -y \
    gcc \
    make \
    libpng-dev \
    libmcrypt-dev \
    libjpeg-dev \
    libfreetype6-dev \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

# Make "npm install" executable on the container.
RUN curl -sL https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get update && apt-get install -y \
    nodejs

RUN a2enmod rewrite

RUN set -ex apk --no-cache add postgresql-dev libpq-dev && \
    apt-get update && apt-get install -y libpq-dev \
    less \
    postgresql \
    && docker-php-ext-install pdo pdo_pgsql pgsql mbstring