
### tag image
docker tag [IMAGE_ID] [AWS_ACCOUNT_ID].dkr.ecr.[AWS_REGION].amazonaws.com/[ECR_REPO]:verity


docker tag [IMAGE_ID] "$(aws sts get-caller-identity --output text --query 'Account').dkr.ecr.$(aws configure get region.amazonaws.com)/hello-world:verity"

docker run --log-driver=awslogs --log-opt awslogs-group=ntno-challenge-docker-logs -p 80:80 281517792612.dkr.ecr.us-east-2.amazonaws.com/hello-world:verity


#install text utility
RUN apt-get -y install gettext-base

COPY region.conf.template /etc/nginx/region.conf.template
RUN /bin/bash -c "envsubst '\$WORKER_PROCESSES \$WORKER_RLIMIT_NOFILE \$WORKER_CONNECTIONS' < /etc/nginx/region.conf.template > /etc/nginx/region.conf" 



prod
50 worker processes
worker_rlimit_nofile of 8192
worker_connections at 4096


dev 
10 worker processes
worker_rlimit_nofile of 4096
worker_connections at 1024 


serve the contents of /var/www/




