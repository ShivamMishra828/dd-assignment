# Use the official Node.js LTS image (lightweight Alpine Linux variant)
FROM node:lts-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json first to optimize Docker caching
COPY package*.json .

# Install project dependencies
RUN npm install

# Copy the rest of the application source code
COPY . .

# Expose the port that the application listens
EXPOSE 8080

# Run the application using Node.js
CMD ["node", "server.js"]
