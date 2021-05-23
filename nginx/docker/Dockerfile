FROM nginx


########## Preprocess ##########
WORKDIR /var/www/html
RUN set -xe
RUN ln -snf /usr/share/zoneinfo/Asia/Tehran /etc/localtime \
        && echo Asia/Tehran > /etc/timezone


########## Install dependencies ##########
RUN apt-get update -y \
        && apt-get upgrade -y


########## Crontab ##########
RUN apt-get install -y --no-install-recommends nano


########## SSL ##########
RUN apt-get install -y --no-install-recommends openssl


########## htpasswd ##########
RUN apt-get install -y apache2-utils


########## Config ##########
RUN sed -i "s/worker_processes  1;/worker_processes  $(grep ^processor /proc/cpuinfo | wc -l);/" /etc/nginx/nginx.conf \
        && sed -i "s/#gzip  on;/gzip  on;/" /etc/nginx/nginx.conf \
        && echo "0 0 * * * nginx -s reload" >> /etc/crontab


########## User ##########
ARG uid=1000
ARG gid=1000
RUN apt-get install -y --no-install-recommends sudo \
        && usermod -u $uid www-data \
        && groupmod -g $gid www-data



########## entrypoint ##########
#COPY entrypoint.sh /
#RUN chmod +x /entrypoint.sh
#ENTRYPOINT ["/entrypoint.sh"]

########## Other ##########
RUN rm -rf /var/lib/apt/lists/*
ENV DOMAIN=localhost
CMD ["nginx", "-g", "daemon off;"]





