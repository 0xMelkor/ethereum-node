FROM nginx
WORKDIR /usr/app

ARG CRYSTAL_MEV_API_KEY
RUN test -n "$CRYSTAL_MEV_API_KEY"

COPY ./nginx/nginx.conf /tmp/crystalmev/nginx.conf

RUN CRYSTAL_MEV_API_KEY=$CRYSTAL_MEV_API_KEY  \
    envsubst '${CRYSTAL_MEV_API_KEY}' < /tmp/crystalmev/nginx.conf > /etc/nginx/conf.d/default.conf

RUN echo $(cat /etc/nginx/conf.d/default.conf)
