FROM nginx
WORKDIR /usr/app

ARG CRYSTAL_MEV_API_KEY
RUN test -n "$CRYSTAL_MEV_API_KEY"

COPY ./nginx/nginx.conf /tmp/crystalmev/nginx.conf
RUN _CRYSTAL_MEV_API_KEY=$CRYSTAL_MEV_API_KEY  envsubst < /tmp/crystalmev/nginx.conf > /etc/nginx/conf.d/default.conf
RUN echo $(cat /etc/nginx/conf.d/default.conf)