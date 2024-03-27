Bitcoin Price Check with AWS Batch
=================================

This project demonstrates the use of AWS Batch to run a Docker application that fetches the current price of Bitcoin. The project includes the following steps:

1. Setting up the AWS Batch environment
2. Creating a Docker image and pushing it to Amazon Elastic Container Registry (ECR)
3. Defining the AWS Batch job
4. Scheduling the job with AWS EventBridge
5. Storing the results in DynamoDB

Prerequisites
-------------

* An AWS account
* The AWS CLI installed and configured
* Docker installed

Setting up the AWS Batch Environment
-----------------------------------

### Step 1: Create a Job Definition

A job definition is a template that contains the information AWS Batch needs to run a job, such as the Docker image to use, the commands to run, and the CPU and memory requirements.

The job definition for this project includes the following information:

* The Docker image to use (e.g. `<ECR_REPO_URI>/bitcoin-price-check:latest`)
* The number of vCPUs and the amount of memory to allocate
* The command to run (e.g. `python app.py`)
* The platform capabilities (e.g. `EC2` or `FARGATE`)
* The VPC configuration (e.g. subnets and security groups)

### Step 2: Create a Job Queue

A job queue is a queue that holds one or more jobs that are waiting to be executed.

The job queue for this project is created with the following properties:

* A name (e.g. `bitcoin-price-check-queue`)
* A compute environment (e.g. `AWSBatch_FargateSPOT`)

### Step 3: Create a Compute Environment

A compute environment is a logical grouping of compute resources that AWS Batch uses to launch and manage your jobs.

The compute environment for this project is created with the following properties:

* A name (e.g. `bitcoin-price-check-env`)
* The type of compute environment (e.g. `FARGATE`)
* An IAM role with the necessary permissions
* The VPC configuration (e.g. subnets and security groups)

### Step 4: Create a Scheduler

The job is scheduled to run every 2 minutes with AWS EventBridge.

The CloudWatch Event for this project includes the following properties:

* A name (e.g. `bitcoin-price-check`)
* A schedule expression (e.g. `rate(2 minutes)`)
* The target (e.g. the ARN of the job queue)

Docker Image
------------

The Docker image for this project includes the following components:

* A base image (e.g. `python:3.8`)
* The necessary dependencies (e.g. `pip install requests`)
* The application code (e.g. `app.py`)

The Dockerfile for this project includes the following steps:

1. Set the working directory
2. Copy the requirements file
3. Install the dependencies
4. Copy the application code
5. Set the command to run

The Docker image is pushed to Amazon Elastic Container Registry (ECR) and is referenced in the job definition.

Storing the Results
-------------------

The results of the job are stored in DynamoDB. The table includes the following properties:

* A primary key (e.g. `timestamp`)
* The price of Bitcoin (e.g. `price`)

The application code writes the results to the DynamoDB table.