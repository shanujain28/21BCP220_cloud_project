
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Docker and Three-Tier Architecture</title>
    <style>
    body {
        font-family: Arial, sans-serif;
        line-height: 1.6;
        margin: 0;
        padding: 0;
        background-color: #f8f9fa;
        color: #333;
    }

    /* Container styling */
    .container {
        max-width: 800px;
        margin: 20px auto;
        padding: 0 20px;
    }

    /* Header hover effect */
    h1:hover,
    h2:hover,
    h3:hover {
        color: #007bff;
        cursor: pointer;
    }
    /* Add some spacing between sections */
    h2 {
        margin-top: 2rem;
    }

    h3 {
        margin-top: 1.5rem;
    }

    /* Style code blocks */
    pre {
        background-color: #f5f5f5;
        padding: 1rem;
        border-radius: 5px;
        overflow-x: auto;
    }

    /* Add some box-shadow for depth */
    pre, p {
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }

    /* Style links */
    a {
        color: #007bff;
        text-decoration: none;
    }

    a:hover {
        text-decoration: underline;
    }

    /* Add some spacing for paragraphs */
    p {
        margin-bottom: 1rem;
    }

    /* Style the footer */
    footer {
        text-align: center;
        margin-top: 2rem;
        padding: 1rem 0;
        background-color: #f5f5f5;
    }

    
</style>
</head>
<body>
    <h1>This blog is created by Swanubhuti jain_21BCP220</h1>
    <h1>Docker and Three-Tier Architecture</h1>

    <h2>Introduction to Docker</h2>
    <p>Docker is a platform for developing, shipping, and running applications in containers. Containers allow developers to package an application with all of its dependencies into a standardized unit, ensuring that it will run consistently on any environment. Docker provides tools for building, deploying, and managing containers, making it easier to develop and deploy applications in various environments.</p>

    <h2>What is Three-Tier Architecture?</h2>
    <p>Three-tier architecture is a software architecture pattern where an application is divided into three logical tiers or layers: the presentation tier, the application logic tier, and the data storage tier. Each tier has a specific role and responsibility:</p>

    <h2>Using Docker in Three-Tier Architecture</h2>

    <h3>1. Database Tier</h3>
    <p>In the database tier, Docker can be used to containerize the database management system (DBMS) such as PostgreSQL, MySQL, or MongoDB. Docker containers provide a lightweight and portable way to run databases, ensuring consistency across different environments.</p>

    <h3>2. Backend Tier</h3>
    <p>For the backend tier, Docker containers can encapsulate the application logic, including the web servers, APIs, and microservices. Developers can package their backend code and dependencies into Docker images, making it easy to deploy and scale the application components independently.</p>

    <h3>3. Frontend Tier</h3>
    <p>In the frontend tier, Docker can be used to package and deploy the presentation layer components such as static web assets, JavaScript frameworks like React or Angular, and web servers like Nginx or Apache. Docker containers enable developers to build and deploy frontend applications with ease, ensuring consistency and reliability.</p>

    <h2>Part 1 - Setting Up the Database Tier with MongoDB</h2>

    <h3>Step 1: Dockerfile for MongoDB</h3>
    <pre><code># Use the official MongoDB image as the base image
FROM mongo:latest

# Set container name with roll number
EXPOSE 27017
    </code></pre>

    <h3>Step 2: Building and Running the Database</h3>
    <pre><code># Build the Docker image for MongoDB
docker build -t mongodb  .

# Run the MongoDB container
docker run -d --name mongodb  -p 27017:27017 mongodb
    </code></pre>

    <h3>Step 3: Connecting MongoDB Container to Network</h3>
    <pre><code># Create a Docker network
docker network create my-network

# Connect MongoDB container to the network
docker network connect my-network mongodb
    </code></pre>

    <h2>Part 2 - Setting Up the Backend Tier with Node.js</h2>

    <h3>Step 1: Dockerfile for Node.js Backend</h3>
    <pre><code># Use the official Node.js image as the base image
FROM node:latest

# Set container name with roll number

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
    </code></pre>

    <h3>Step 2: Building and Running Node.js Backend Container</h3>
    <pre><code># Build the Docker image for Node.js backend
docker build -t nodejs-backend .

# Run the Node.js backend container
docker run -d --name nodejs-backend -p 5000:5000 --network my-network nodejs-backend
    </code></pre>

    <h2>Part 3 - Setting Up the Frontend Tier with React</h2>

    <h3>Step 1: Dockerfile for React Frontend</h3>
    <pre><code># Use the official Node.js image as the base image for building React app
FROM node:latest as build

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
    </code></pre>

    <h3>Step 2: Building and Running React Frontend Container</h3>
    <pre><code># Build the Docker image for React frontend
docker build -t react-frontend  .

# Run the React frontend container
docker run -d --name react-frontend -p 80:80 --network my-network react-frontend 
    </code></pre>

    <h2>Conclusion</h2>
    <p>Docker simplifies the development, deployment, and management of applications based on the three-tier architecture. By using Docker containers for each tier, developers can achieve consistency, portability, and scalability in their applications, leading to faster development cycles and more efficient deployment processes.</p>

    <h2>Thank you for visiting the documentation</h2>
</body>
</html>
