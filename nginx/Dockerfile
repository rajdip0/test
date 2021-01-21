FROM nginx:1.19.0-alpine

RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d
ARG CERTBOT_EMAIL=info@domain.com
ARG DOMAIN_LIST
RUN  apt update \
      && apt install -y cron bash wget \
      && wget -O /usr/local/bin/certbot-auto https://dl.eff.org/certbot-auto \
      && chown root /usr/local/bin/certbot-auto \
      && chmod 0755 /usr/local/bin/certbot-auto \
      && certbot-auto certonly --standalone --agree-tos -m "${CERTBOT_EMAIL}" -n -d ${DOMAIN_LIST} \
      && rm -rf /var/lib/apt/lists/* \
      && echo "@monthly /usr/local/bin/certbot-auto renew --nginx >> /var/log/cron.log 2>&1" >/etc/cron.d/certbot-renew \
      && crontab /etc/cron.d/certbot-renew
VOLUME /etc/letsencrypt
CMD [ "sh", "-c", "cron && nginx -g 'daemon off;'" ]