# Use official Node.js image as base
FROM node:16-alpine

# Set working directory in the container
WORKDIR /app

# Copy package.json and package-lock.json to work directory
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code to work directory
COPY . .

# Expose port 5000 (or whichever port your backend listens on)
EXPOSE 5000

# Command to run the backend server
CMD ["node","app.js"]