
### tag image
docker tag [IMAGE_ID] [AWS_ACCOUNT_ID].dkr.ecr.[AWS_REGION].amazonaws.com/[ECR_REPO]:verity


docker tag [IMAGE_ID] "$(aws sts get-caller-identity --output text --query 'Account').dkr.ecr.$(aws configure get region.amazonaws.com)/hello-world:verity"

docker run --log-driver=awslogs --log-opt awslogs-group=ntno-challenge-docker-logs -p 80:80 281517792612.dkr.ecr.us-east-2.amazonaws.com/hello-world:verity