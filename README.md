# docker-config-example

## Step 1) Build
### development image 
`docker build hello-verity -t hello-verity --build-arg ENVIRON=dev`  
or  
`export ENVIRON=dev && docker build hello-verity -t hello-verity --build-arg ENVIRON=$ENVIRON`  


### prod image
`docker build hello-verity -t hello-verity --build-arg ENVIRON=prod`  
or  
`export ENVIRON=prod && docker build hello-verity -t hello-verity --build-arg ENVIRON=$ENVIRON`  

*Note: image will default to the dev configuration if no build argument is specified*

## Step 2) Deploy
### 2.a - prerequisites
* aws account with existing elastic container registry repository 
* awscli installed
* awscli configured with user who has read/write access to the repository

### 2.b - tag the newly built image
use the following command to tag your new image (built in the previous step) where

* IMAGE_ID is the docker id for the image,
* AWS_ACCOUNT_ID is your aws account id number,
* AWS_REGION is your default aws region,
* ECR_REPO is the name of the elastic container registry repository in your account, and
* LABEL is a custom label

`docker tag [IMAGE_ID] [AWS_ACCOUNT_ID].dkr.ecr.[AWS_REGION].amazonaws.com/[ECR_REPO]:[LABEL]`

if you are unsure of any of these inputs you can use the following commands to help find out what the correct values are: 

* IMAGE_ID
  * `docker image ls`
* AWS_ACCOUNT_ID
  * `$(aws sts get-caller-identity --output text --query 'Account')`
* AWS_REGION
  * `$(aws configure get region.amazonaws.com)`
* ECR_REPO
  * check the aws console or refer to the code which deployed the repository 
* LABEL
  * you can choose this one

*Note:* keep the dollar sign and the parentheses in the above commands


example:  
`docker tag 56b6386818ba 987654321.dkr.ecr.us-east-2.amazonaws.com/hello-world:verity`


### 2.c - push the image 
use the following command to push the image to your elastic container registry repository

`docker push [AWS_ACCOUNT_ID].dkr.ecr.[AWS_REGION].amazonaws.com/[ECR_REPO]:[LABEL]`

example:  
`docker push 987654321.dkr.ecr.us-east-2.amazonaws.com/hello-world:verity`


## Step 3) Run image as a container
### 3.a - run image locally with port 80 open

`docker run -p 80:80 -d 987654321.dkr.ecr.us-east-2.amazonaws.com/hello-world:verity`

### 3.b - run image in EC2 or ECS and configure cloudwatch logging
*Prerequisite: you must already have a log group provisioned and an ec2 instance with the proper permissions*

`docker pull 987654321.dkr.ecr.us-east-2.amazonaws.com/hello-world:verity` 

`docker run --log-driver=awslogs --log-opt awslogs-group=[YOUR_LOG_GROUP_NAME_HERE] -p 80:80 -d  987654321.dkr.ecr.us-east-2.amazonaws.com/hello-world:verity`


## Future Enhancements
* continue to research how to swap out environment variables in an easy / robust way
  * started looking into the utility "gettext-base" which looked promising but there is probably a better way to do it using docker or docker-compose.  I will need to read the docs further
* automated infrastructure deployment of ec2 instance or ecs cluster
  * see work in progress [here](https://github.com/ntno/ntno-challenge/blob/master/infrastructure/cloudformation/cft/deploy-hello-world-app.yml)
* automated build/deploy pipeline on aws codepipeline
  * see work in progress [here](https://github.com/ntno/ntno-challenge/blob/master/infrastructure/cloudformation/cft/pipeline.yml)