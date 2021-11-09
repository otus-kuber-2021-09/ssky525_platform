FROM nginx:1.21.1

MAINTAINER Pavel Petrovichev "pavel.petrovichev@gmail.com"

#ARG nginx_gid=1001
#ARG nginx_uid=1001
RUN mkdir /app

RUN usermod -u 1001 nginx
#RUN groupmod -g 1001 nginx
COPY app/homework.html /app/


#COPY default.conf /etc/nginx/conf.d/
#RUN chown -R nginx:nginx /app 
#RUN chown -R nginx:nginx /etc/nginx


#COPY nginx.conf /etc/nginx/
COPY default.conf /etc/nginx/conf.d/
#COPY docker-entrypoint.sh /

#USER nginx
EXPOSE 8000
#ENTRYPOINT ["/docker-entrypoint.sh"]
#CMD ["nginx", "-g", "daemon off;"]
