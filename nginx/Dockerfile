FROM nginx
WORKDIR /usr/app

ARG RPC_USERNAME
ARG RPC_PASSWORD
RUN test -n "$RPC_USERNAME"
RUN test -n "$RPC_PASSWORD"

# Create basic auth credentials
RUN apt-get update
RUN yes | apt-get install apache2-utils
RUN mkdir /etc/apache2
RUN touch /etc/apache2/.htpasswd
RUN htpasswd -b /etc/apache2/.htpasswd $RPC_USERNAME $RPC_PASSWORD

COPY ./nginx/nginx.conf /etc/nginx/conf.d/default.conf