FROM arm64v8/php:7.4-cli
RUN apt-get update && apt-get install -y git \
    && ( \
    cd /tmp \
    && mkdir librdkafka \
    && cd librdkafka \
    && git clone https://github.com/edenhill/librdkafka.git . \
    && ./configure \
    && make \
    && make install \
    ) 
RUN pecl install redis \
    rdkafka \
    rdkafka \
    && docker-php-ext-enable redis rdkafka \
    && docker-php-ext-install \
    mysqli pdo pdo_mysql \
    pcntl \
    sockets
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/
RUN install-php-extensions gd \
    gRPC \
    imagick
RUN cd /tmp && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && rm -rf /tmp/*