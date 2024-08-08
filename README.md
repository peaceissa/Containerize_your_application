# Containerizing Your Cloud: Mastering AWS and Docker for Seamless Deployment

## Project Overview

This project is a hands-on exercise designed to strengthen your skills in Docker and AWS ECS by guiding you through the process of containerizing an application, testing it locally, uploading it to Docker Hub, and deploying it on AWS ECS. The application is a simple web server that returns the message "Hello your_group_name" when accessed via HTTP.

## Table of Contents

- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Docker Container Creation](#docker-container-creation)
  - [Step 1: Create the Dockerfile](#step-1-create-the-dockerfile)
  - [Step 2: Build the Docker Image](#step-2-build-the-docker-image)
  - [Step 3: Run and Test Locally](#step-3-run-and-test-locally)
- [Pushing to Docker Hub](#pushing-to-docker-hub)
- [Deploying to AWS ECS](#deploying-to-aws-ecs)
  - [Step 1: Create an ECS Cluster](#step-1-create-an-ecs-cluster)
  - [Step 2: Create a Task Definition](#step-2-create-a-task-definition)
  - [Step 3: Create and Run an ECS Service](#step-3-create-and-run-an-ecs-service)
- [Justification of Design Choices](#justification-of-design-choices)
- [Accessing the Application](#accessing-the-application)
- [Conclusion](#conclusion)
- [References](#references)

## Prerequisites

Before you start, ensure you have the following:

- **Docker** installed on your local machine.
- **AWS CLI** configured with your AWS credentials.
- **AWS Account** with sufficient permissions to create and manage ECS resources.
- **Docker Hub Account** for pushing your Docker image.

## Docker Container Creation

### Step 1: Create the Dockerfile

Create a `Dockerfile` to define the environment and the application:

```dockerfile
# Use an official Node.js runtime as the base image
FROM node:14

# Set the working directory
WORKDIR /usr/src/app

# Copy the package.json and install dependencies
COPY package*.json ./
RUN npm install

# Copy the application code
COPY . .

# Expose port 8080
EXPOSE 8080

# Define the command to run the application
CMD ["node", "app.js"]
```

### Step 2: Build the Docker Image

Build the Docker image using the following command:

```bash
docker build -t your_group_name/hello-app .
```

### Step 3: Run and Test Locally

Run the Docker container locally to test the application:

```bash
docker run -p 8080:8080 your_group_name/hello-app
```

Test the application by navigating to `http://localhost:8080` in your web browser. You should see "Hello your_group_name".

## Pushing to Docker Hub

1. Log in to Docker Hub:

   ```bash
   docker login
   ```

2. Tag your Docker image:

   ```bash
   docker tag your_group_name/hello-app your_dockerhub_username/hello-app
   ```

3. Push the Docker image to Docker Hub:

   ```bash
   docker push your_dockerhub_username/hello-app
   ```

## Deploying to AWS ECS

### Step 1: Create an ECS Cluster

1. Open the AWS Management Console.
2. Navigate to the **ECS** service.
3. Create a new ECS Cluster. Choose the appropriate cluster type based on your infrastructure needs (e.g., **Fargate** for serverless).

### Step 2: Create a Task Definition

1. In the ECS console, create a new Task Definition.
2. Choose the **Fargate** launch type.
3. Define the container details, including the Docker image URL (from Docker Hub) and the necessary environment variables.
4. Set the memory and CPU requirements.

### Step 3: Create and Run an ECS Service

1. Create a new service using the Task Definition.
2. Choose the desired number of tasks to run.
3. Select a load balancer or allow ECS to manage the network configuration.
4. Deploy the service.

## Justification of Design Choices

### Container Choice

- **Node.js**: Chosen for its lightweight nature and ability to handle HTTP requests efficiently. The Node.js runtime is well-suited for simple web applications like this one.

### AWS ECS & Fargate

- **ECS**: Chosen for its integration with AWS services, scalability, and ease of management.
- **Fargate**: Opted for a serverless experience, reducing overhead for managing EC2 instances and providing cost efficiency with pay-as-you-go pricing.

## Accessing the Application

Once deployed, the application should be accessible via the public IP or domain name associated with the ECS service. Visit the URL in your browser to see the "Hello your_group_name" message.

## Conclusion

This project demonstrated how to containerize an application, test it locally, push it to Docker Hub, and deploy it on AWS ECS. The choices made were based on balancing performance, cost, and scalability to create an efficient and effective deployment pipeline.

## References

- [Docker Documentation](https://docs.docker.com/)
- [AWS ECS Documentation](https://docs.aws.amazon.com/ecs/)
- [Node.js Documentation](https://nodejs.org/en/docs/)

