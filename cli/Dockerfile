FROM  php:8.2-cli

ENV WORDPRESS_CLI_VERSION 2.8.1

# Install requirements for wp-cli support
RUN apt-get update && \
    apt-get install --no-install-recommends -yqq \
        sudo \
        less \
        default-mysql-client \
        libjpeg-dev \
        libpng-dev 

RUN docker-php-ext-configure gd  && \
    docker-php-ext-install gd mysqli opcache

# set recommended PHP.ini settings
# see https://secure.php.net/manual/en/opcache.installation.php
RUN { \
		echo 'opcache.memory_consumption=128'; \
		echo 'opcache.interned_strings_buffer=8'; \
		echo 'opcache.max_accelerated_files=4000'; \
		echo 'opcache.revalidate_freq=2'; \
		echo 'opcache.fast_shutdown=1'; \
		echo 'opcache.enable_cli=1'; \
} > /usr/local/etc/php/conf.d/opcache-recommended.ini 

# Install needed php extensions: ldap
#
RUN apt-get install libldap2-dev -y && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap

RUN mkdir -p /var/www/html && \
    chown -R www-data:www-data /var/www/html

RUN curl -L https://github.com/wp-cli/wp-cli/releases/download/v${WORDPRESS_CLI_VERSION}/wp-cli-${WORDPRESS_CLI_VERSION}.phar \
    -o /usr/local/bin/wp-cli.phar &&  \
    chmod +x /usr/local/bin/wp-cli.phar && \
    ln -s /usr/local/bin/wp-cli.phar /bin/wp && \
    wp --allow-root --version

# Cleanup
RUN rm -rf /var/lib/apt/lists/*

COPY docker-entrypoint.sh /docker-entrypoint.sh
RUN  chmod +x /docker-entrypoint.sh

WORKDIR /var/www/html

VOLUME /var/www/html

USER www-data

ENTRYPOINT [ "/docker-entrypoint.sh" ]
CMD ["wp", "shell"]
