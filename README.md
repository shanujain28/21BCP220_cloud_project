# Docker and Three-Tier Architecture

## Introduction to Docker

Docker is a platform for developing, shipping, and running applications in containers. Containers allow developers to package an application with all of its dependencies into a standardized unit, ensuring that it will run consistently on any environment. Docker provides tools for building, deploying, and managing containers, making it easier to develop and deploy applications in various environments.

## What is Three-Tier Architecture?

Three-tier architecture is a software architecture pattern where an application is divided into three logical tiers or layers: the presentation tier, the application logic tier, and the data storage tier. Each tier has a specific role and responsibility:

## Using Docker in Three-Tier Architecture

Docker is particularly useful in implementing and deploying applications based on the three-tier architecture:

### 1. Database Tier

In the database tier, Docker can be used to containerize the database management system (DBMS) such as PostgreSQL, MySQL, or MongoDB. Docker containers provide a lightweight and portable way to run databases, ensuring consistency across different environments.

### 2. Backend Tier

For the backend tier, Docker containers can encapsulate the application logic, including the web servers, APIs, and microservices. Developers can package their backend code and dependencies into Docker images, making it easy to deploy and scale the application components independently.

### 3. Frontend Tier

In the frontend tier, Docker can be used to package and deploy the presentation layer components such as static web assets, JavaScript frameworks like React or Angular, and web servers like Nginx or Apache. Docker containers enable developers to build and deploy frontend applications with ease, ensuring consistency and reliability.


# Part 1 - Setting Up the Database Tier with MongoDB

In this part of the tutorial, we will set up the database tier using MongoDB.

## Step 1: Dockerfile for MongoDB

```dockerfile
# Use the official MongoDB image as the base image
FROM mongo:latest

# Set container name with roll number
ENV MONGO_CONTAINER_NAME="mongodb "
```

## Step 2: building and running the database

```dockerfile
# Build the Docker image for MongoDB
docker build -t mongodb  .

# Run the MongoDB container
docker run -d --name mongodb  -p 27017:27017 mongodb

```

## Step 3: Connecting MongoDB Container to Network
```dockerfile
# Create a Docker network
docker network create my-network

# Connect MongoDB container to the network
docker network connect my-network mongodb
```

---
layout: part2
title: Part 2 - Setting Up the Backend Tier
permalink: /part2/
---

<!-- Content for Docker part 2 -->

# Part 2 - Setting Up the Backend Tier with Node.js

In this part of the tutorial, we will set up the backend tier using Node.js.

## Step 1: Dockerfile for Node.js Backend

```dockerfile
# Use the official Node.js image as the base image
FROM node:latest

# Set container name with roll number
ENV NODE_CONTAINER_NAME="nodejs-backend"

# Create and set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy backend source code
COPY . .

# Expose port 5000
EXPOSE 5000

# Command to run the backend server
CMD ["node", "server.js"]
```


## Step 2: Building and Running Node.js Backend Container

``` dockerfile
# Build the Docker image for Node.js backend
docker build -t nodejs-backend .

# Run the Node.js backend container
docker run -d --name nodejs-backend -p 5000:5000 --network my-network nodejs-backend
```

---
layout: part3
title: Part 3 - Setting Up the Frontend Tier
permalink: /part3/
---

<!-- Content for Docker part 3 -->

# Part 3 - Setting Up the Frontend Tier with React

In this part of the tutorial, we will set up the frontend tier using React.

## Step 1: Dockerfile for React Frontend

```dockerfile
# Use the official Node.js image as the base image for building React app
FROM node:latest as build

# Set container name with roll number
ENV REACT_CONTAINER_NAME="react-frontend"

# Create and set working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy frontend source code
COPY . .

# Build React app
RUN npm run build

# Use NGINX for serving React app
FROM nginx:alpine

# Copy build files from build stage
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Command to start NGINX
CMD ["nginx", "-g", "daemon off;"]
```

## Step 2: Building and Running React Frontend Container
 ``` dockerfile
# Build the Docker image for React frontend
docker build -t react-frontend  .

# Run the React frontend container
docker run -d --name react-frontend -p 80:80 --network my-network react-frontend 
```

## Conclusion

Docker simplifies the development, deployment, and management of applications based on the three-tier architecture. By using Docker containers for each tier, developers can achieve consistency, portability, and scalability in their applications, leading to faster development cycles and more efficient deployment processes.

## Thank you for visiting the documentation

## By Krupa Siddh - 21BCP345