FROM nginx
WORKDIR /usr/app

ARG CRYSTAL_MEV_API_KEY
RUN test -n "$CRYSTAL_MEV_API_KEY"

RUN _CRYSTAL_MEV_API_KEY=$CRYSTAL_MEV_API_KEY  envsubst < ./nginx/nginx.conf > ./nginx/nginx_main.conf
RUN cat ./nginx/nginx_main.conf 

COPY ./nginx/nginx_main.conf /etc/nginx/conf.d/default.conf