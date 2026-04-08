# Particle41 DevOps Challenge

This repository contains my solution for the DevOps challenge, covering:

* Task 1: SimpleTimeService (Application + Docker + Kubernetes)
* Task 2: AWS Infrastructure using Terraform (EKS)
* Branch => Master

---

# Task 1 – SimpleTimeService

# Overview

I built a small microservice that returns the current timestamp and the client IP address in JSON format.

# Example Response

```json
{
  "timestamp": "2026-04-07T12:00:00",
  "ip": "10.0.0.1"
}
```

---

##  What I Did

* Created a simple web server (Python + Flask)
* Dockerized the application
* Ensured the container runs as a **non-root user**
* Pushed the image to DockerHub
* Created Kubernetes Deployment and Service YAML

---

## Docker Image

The image is publicly available:

docker pull knikhil999/simple-time-service:latest

##  How to Run Task 1

### Prerequisites

* Kubernetes cluster (Minikube / EKS / any cluster)
* kubectl installed



### Deploy
cd app

kubectl apply -f microservice.yml

### Verify

kubectl get pods

kubectl get svc


### Test
kubectl port-forward svc/simple-time-service 8080:80

curl http://localhost:8080

---
## CICD


This project includes a CI/CD pipeline using GitHub Actions.

Automatically triggers on push to master branch

Builds Docker image

Pushes image to DockerHub

Workflow File Location

.github/workflows/docker-publish.yml

Required Secrets

To enable Docker push, configure the following in GitHub:

DOCKER_USERNAME

DOCKER_PASSWORD

How to Add Secrets

Go to Repository → Settings

Secrets and variables → Actions

Add the above secrets

How to Trigger

git add .

git commit -m "trigger pipeline"

git push origin master

Then check:

GitHub → Actions tab


##  Notes

* Deployment and Service are defined in a single YAML file
* Service type is **ClusterIP** (as required)
* Works with a single command: `kubectl apply -f microservice.yml`
* Container runs as non-root user

#  Task 2 – Terraform (AWS EKS)

## Overview

I used Terraform to create AWS infrastructure including:

* VPC
* Subnets (public + private)
* EKS cluster
* Node group
* Required IAM Permissions

---

## What I Did

* Used Terraform modules for VPC and EKS
* Created:

  * 2 public subnets
  * 2 private subnets
* Deployed EKS cluster inside the VPC
* Configured node group:

  * 2 nodes
  * instance type: `m6a.large`
  * nodes in private subnets
* Used variables and tfvars for clean configuration
* Kept setup simple so it runs with `terraform apply`

---

## Prerequisites

* AWS account
* AWS CLI installed
* Terraform installed


## Configure AWS

aws configure


Provide:

* Access key
* Secret key
* Region → `ap-south-1`

---

## How to Run Task 2
cd terraform

terraform init
terraform plan
terraform apply

##  What Gets Created

* VPC (10.0.0.0/16)
* 2 public subnets
* 2 private subnets
* NAT Gateway
* EKS cluster
* Node group with 2 EC2 instances (m6a.large)
* Nodes placed in private subnets

---

##  How to Verify

Using AWS Console:

* Check VPC and subnets
* Check EKS cluster status
* Check node group
* Verify EC2 instances (2 nodes)

---

##  Notes

* No credentials are stored in the repository
* Terraform uses local state for simplicity
* Setup is designed so anyone can run `terraform apply` directly

---

#  Summary

* Task 1: Built and deployed a containerized microservice on Kubernetes
* Task 2: Provisioned AWS infrastructure using Terraform
* Both tasks are tested and reproducible using standard commands

