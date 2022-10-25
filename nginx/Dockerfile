FROM nginx
WORKDIR /usr/app

ARG AUTH_USERNAME
ARG AUTH_PASSWORD
RUN test -n "$AUTH_USERNAME"
RUN test -n "$AUTH_PASSWORD"

# Create basic auth credentials
RUN apt-get update
RUN yes | apt-get install apache2-utils
RUN mkdir /etc/apache2
RUN touch /etc/apache2/.htpasswd
RUN htpasswd -b /etc/apache2/.htpasswd $AUTH_USERNAME $AUTH_PASSWORD

COPY ./k8s/nginx/nginx.conf /etc/nginx/conf.d/default.conf