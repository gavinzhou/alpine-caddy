FROM alpine:3.4
MAINTAINER Gavin Zhou <gavin.zhou@gmail.com>

ENV CADDY_VERSION=0.9.2 \
 PLUGINS=jwt

RUN set -x \
 && apk add --no-cache --virtual .gosu-deps tar curl make gcc musl-dev \
 && curl -sSL https://github.com/Yelp/dumb-init/archive/master.tar.gz | tar -xz \
 && cd dumb-init-master && gcc -std=gnu99 -s -Wall -Werror -O3 -o /sbin/dumb-init /dumb-init-master/dumb-init.c \
 && curl --silent --show-error --fail --location \
      --header "Accept: application/tar+gzip, application/x-gzip, application/octet-stream" -o - \
      "https://caddyserver.com/download/build?os=linux&arch=amd64&features=${PLUGINS}" \
    | tar --no-same-owner -C /usr/bin/ -xz caddy \
 && chmod 0755 /usr/bin/caddy \
 && apk del .gosu-deps

EXPOSE 2015

COPY Caddyfile /etc/Caddyfile
COPY docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["caddy", "-conf", "/etc/Caddyfile"]
