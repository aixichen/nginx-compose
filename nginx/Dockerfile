FROM nginx:1.13-alpine
RUN mkdir -p /etc/nginx/conf.d/ssl
COPY conf.d /etc/nginx/conf.d
COPY ssl /etc/nginx/ssl
USER root
