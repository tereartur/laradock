FROM nginx:alpine

MAINTAINER Mahmoud Zalt <mahmoud@zalt.me>

ADD nginx.conf /etc/nginx/
COPY sites/*.conf /etc/nginx/sites-available/

# install openssl
RUN apk add --no-cache openssl

# create a folder for the keys
RUN mkdir /etc/nginx/ssl 2> /dev/null

# generate the keys for your local domain
RUN openssl genrsa -out "/etc/nginx/ssl/api.app.key" 2048 \
    && openssl req -new -key "/etc/nginx/ssl/api.app.key" -out "/etc/nginx/ssl/api.app.csr" -subj "/CN=api.app/O=Begin/C=EE" \
    && openssl x509 -req -days 365 -in "/etc/nginx/ssl/api.app.csr" -signkey "/etc/nginx/ssl/api.app.key" -out "/etc/nginx/ssl/api.app.crt"

ARG PHP_UPSTREAM=php-fpm

RUN apk update \
    && apk upgrade \
    && apk add --no-cache bash \
    && adduser -D -H -u 1000 -s /bin/bash www-data \
    && rm /etc/nginx/conf.d/default.conf \
    && echo "upstream php-upstream { server ${PHP_UPSTREAM}:9000; }" > /etc/nginx/conf.d/upstream.conf

CMD ["nginx"]

EXPOSE 80 443
