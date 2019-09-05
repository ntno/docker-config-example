FROM nginx

#back up original configurations
RUN cp /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf.backup
RUN cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup

#link logs to stdout/stderr
RUN /bin/bash -c "ln -sf /dev/stdout /var/log/nginx/access.log"
RUN /bin/bash -c "ln -sf /dev/stderr /var/log/nginx/error.log"

#create user for hello-verity site
RUN adduser --system --no-create-home --shell /bin/false --group --disabled-login www-hello-verity

#copy the configuration
COPY nginx.conf /etc/nginx

#copy the site content
COPY site /var/www

#test the configuration
RUN /bin/bash -c "nginx -t"
