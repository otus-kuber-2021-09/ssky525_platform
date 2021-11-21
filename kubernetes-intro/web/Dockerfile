FROM nginx:1.21.1

MAINTAINER Pavel Petrovichev "pavel.petrovichev@gmail.com"

RUN mkdir /app
RUN usermod -u 1001 nginx

COPY homework.html /app
COPY default.conf /etc/nginx/conf.d/

EXPOSE 8000
