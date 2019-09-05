# docker-config-example

### build image
docker build hello-verity -t hello-verity


prod
50 worker processes
worker_rlimit_nofile of 8192
worker_connections at 4096


dev 
10 worker processes
worker_rlimit_nofile of 4096
worker_connections at 1024 



serve the contents of /var/www/.