# AWS-Docker-Simple-Wesbite
 
This is a simple repo to undertand the use of Docker Containerization using a simple website that will run on NGINX and be deployed on AWS using ECS services

## What is Docker containerization?
Package Software into Standardized Units for Development, Shipment and Deployment.

A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

# AWS

## EC2
- Create an EC2 Ubuntu instance with port 22 for ssh open
- SSH to the instancce
- Crete a project dir and cretae a simple HTML to display 

__Run CMD__

```sh
sudo su
apt update && apt install docker.io
mkdir prohectapp
cd projectapps/
vim index.html
#create a simple HTML
vim dockerfile
#We nedd to use the NGINX image and copy the current index file to /usr/share/nginx/html/
```

__Install Required Plugins__

```sh
sudo su
apt update && apt install awscli
```
 

## Docker Image Creation

__Install Required Plugins__

```sh
sudo su
#in the same dir, build docker image
docker build -t html-nginx-website:v1
#after success verify by running 
docker images
#run the image
docker run -it -d -p 80:80 html-nginx-website:v1
#verify the site 
curl localhost
#docker processes running
docker ps
#close all images
docker rm -f $(docker ps -a -q)
```
 
## ECR

As we will be using docker to containerize our Website we can either use DockerHub or AWS ECR to register our image
For now we will use AWS ECR to register our repo
For on AWS create an ECR repo
Copy the URI

SSH to EC2 instance add the repo info
```sh
sudo su
#copy/paste the Push commands for docker-repo
aws ecr get-login-password --region us-... | docker login --username <login> --password-stdin .....amazonaws.co
#tag the docker image to the AWS repo
docker tag html-nginx-website:v1 499266875389.dkr.ecr.us-east-1.amazonaws.com/docker-repo
#push the new tagged iamge to aws repo
docker push 499266875389.dkr.ecr.us-east-1.amazonaws.com/docker-repo
```

## ECS

You can now create a Cluste using Farget or EC2 and create 3 or more replicas of the docker container

