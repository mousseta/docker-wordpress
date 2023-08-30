FROM wordpress:fpm
RUN apt-get update
RUN pecl install memcache
RUN docker-php-ext-enable memcache

# ENTRYPOINT resets CMD
ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["php-fpm"]
