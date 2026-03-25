ARG BUILD_FROM=ghcr.io/hassio-addons/base:20.0.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell for clean pipe error handling
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Copy patches into the image
COPY patches/*.patch /usr/src/

# define NPM version
ARG NGINX_PROXY_MANAGER_VERSION="v2.14.0"

# Build dependencies and runtime packages install
# docker run --rm alpine:3.23.3 sh -c "apk update -q && apk list %dependency% e.g. libpq"
# hadolint ignore=DL3003,DL3042
RUN \
    apk add --no-cache --virtual .build-dependencies \
    build-base=0.5-r3 \
    git=2.52.0-r0 \
    libffi-dev=3.5.2-r0 \
    openssl-dev=3.5.5-r0 \
    patch=2.8-r0 \
    python3-dev=3.12.12-r0 \
    \
    && apk add --no-cache \
    apache2-utils=2.4.66-r0 \
    certbot=5.1.0-r0 \
    libcap=2.77-r0 \
    logrotate=3.22.0-r0 \
    nginx-mod-stream=1.28.2-r1 \
    nginx=1.28.2-r1 \
    nodejs=24.14.1-r0 \
    npm=11.11.0-r0 \
    openssl=3.5.5-r0 \
    py3-pip=25.1.1-r1 \
    python3=3.12.12-r0 \
    yarn=1.22.22-r1 \
    \
    # NPM Source download and extract
    && curl -J -L -o /tmp/nginxproxymanager.tar.gz \
    "https://github.com/NginxProxyManager/nginx-proxy-manager/archive/${NGINX_PROXY_MANAGER_VERSION}.tar.gz" \
    && mkdir /app \
    && tar zxvf /tmp/nginxproxymanager.tar.gz --strip 1 -C /app \
    \
    # --------------------------------------------------------
    # DYNAMISCHES PATCHEN (Ersetzt den alten patches/ Ordner)
    # --------------------------------------------------------
    # 1. Pfade von /data/ auf /config/ umbiegen
    && find /app/backend /app/global /app/docker/rootfs -type f -exec sed -i 's#/data/#/config/#g' {} + \
    # 2. Logs auf Docker-Stdout (/proc/1/fd/1) umleiten
    && find /app/backend/templates /app/docker/rootfs -type f -name "*.conf" -exec sed -i -E 's#access_log /config/logs/[^ ]+ #access_log /proc/1/fd/1 #g' {} + \
    && find /app/backend/templates /app/docker/rootfs -type f -name "*.conf" -exec sed -i -E 's#error_log /config/logs/[^ ]+ #error_log /proc/1/fd/1 #g' {} + \
    # 3. Certbot Plugin Installation fixen
    && sed -i 's#\. /opt/certbot/bin/activate && pip install --no-cache-dir#pip install#g' /app/backend/lib/certbot.js \
    && sed -i 's#  && deactivate##g' /app/backend/lib/certbot.js \
    # 4. Cloudflare Dependency Conflict beheben
    && sed -i 's/cloudflare==4.0.\*/cloudflare/g' /app/global/certbot-dns-plugins.json \
    # 5. Node-Sass durch aktuelles Sass ersetzen
    && sed -i 's/"node-sass": "^9.0.0"/"sass": "^1.85.0"/' /app/frontend/package.json \
    && sed -i "s/'sass-loader'/{ loader: 'sass-loader', options: { implementation: require('sass') } }/" /app/frontend/webpack.config.js \
    # --------------------------------------------------------
    \
    # Frontend Build (with safe scope for NODE_OPTIONS)
    && cd /app/frontend \
    && yarn install --ignore-engines --network-timeout 100000 \
    && NODE_OPTIONS="--openssl-legacy-provider" yarn build --ignore-engines \
    && rm -rf node_modules \
    \
    && mkdir -p /opt/nginx-proxy-manager/frontend \
    && cp -r /app/frontend/dist/. /opt/nginx-proxy-manager/frontend/ \
    \
    # Backend Build (only Production-Dependencies)
    && cd /app/backend \
    && yarn install --production \
    && cp -r /app/backend/. /opt/nginx-proxy-manager/ \
    \
    # setup Nginx and configs 
    && rm -f -r /etc/nginx \
    && cp -r /app/docker/rootfs/etc/nginx /etc/nginx \
    && rm -f /etc/nginx/conf.d/dev.conf \
    \
    && cp /app/docker/rootfs/etc/letsencrypt.ini /etc \
    && cp /app/docker/rootfs/etc/logrotate.d/nginx-proxy-manager /etc/logrotate.d \
    \
    # runtime configurations patches
    && sed -i "s#user npm;#user root;#" /etc/nginx/nginx.conf \
    && sed -i "s#root /app/frontend;#root /opt/nginx-proxy-manager/frontend;#" /etc/nginx/conf.d/production.conf \
    && sed -i "s#table.string('id').notNull().primary();#table.string('id', 32).notNull().primary();#" /opt/nginx-proxy-manager/migrations/20190227065017_settings.js \
    \
    && mkdir -p /run/nginx \
    \
    # clean up system
    && apk del --purge .build-dependencies \
    && yarn cache clean \
    && rm -f -r \
    /app \
    /etc/init.d/nginx \
    /etc/logrotate.d/nginx \
    /root/.cache \
    /root/.node-gyp \
    /tmp/.[!.]* \
    /tmp/* \
    /usr/lib/node_modules \
    /usr/local/share/.cache \
    /var/lib/nginx \
    /var/log/nginx \
    /var/tmp/nginx \
    /var/www

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels for Home Assistant
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
