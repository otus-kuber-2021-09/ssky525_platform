FROM nginx:1.21.3

MAINTAINER Pavel Petrovichev "pavel.petrovichev@gmail.com"

ARG nginx_gid=1001
ARG nginx_uid=1001


RUN mkdir /app

COPY web/nginx.conf /etc/nginx/
COPY web/app/homework.html /app
RUN usermod -u $nginx_uid -o nginx
RUN groupmod -g $nginx_gid -o nginx
RUN chown -R nginx:nginx /app 

EXPOSE 8000
CMD ["nginx", "-g", "daemon off;"]
