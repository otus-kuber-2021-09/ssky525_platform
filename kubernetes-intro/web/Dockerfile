FROM nginx:1.21.3

MAINTAINER Pavel Petrovichev "pavel.petrovichev@gmail.com"

ARG nginx_gid=1001
ARG nginx_uid=1001


RUN mkdir /app

COPY nginx.conf /etc/nginx/
COPY app/homework.html /app
RUN usermod -u $nginx_uid -o nginx           && \
    groupmod -g $nginx_gid -o nginx          && \
    chown -R nginx:nginx /app 
#CMD ["/bin/bash", "-c", "systemctl start nginx"]

EXPOSE 8000
#COPY ["test.html", "/app/test.html"]
#COPY ["entrypoint.sh", "/root/entrypoint.sh"]
#ENTRYPOINT /root/entrypoint.sh
CMD ["nginx", "-g", "daemon off;"]
