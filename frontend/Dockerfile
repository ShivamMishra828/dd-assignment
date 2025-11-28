#  Stage 1: Build Angular App

# Use Node.js LTS image (Alpine variant for smaller image size)
FROM node:lts-alpine as builder

# Set working directory inside the container
WORKDIR /app

# Copy only package.json and package-lock.json for efficient Docker caching
COPY package*.json .

# Install project dependencies
RUN npm install

# Copy the rest of the Angular project files
COPY . .

# Build the Angular application in production mode
RUN npm run build --prod


#  Stage 2: Serve with Nginx

# Use the lightweight Nginx image for serving static files
FROM nginx:alpine

# Copy the built Angular application from the builder stage to Nginx's public directory
COPY --from=builder /app/dist/angular-15-crud /usr/share/nginx/html

# Expose port 80 for serving the Angular app
EXPOSE 80

# Start Nginx in the foreground (required for container runtime)
CMD ["nginx", "-g", "daemon off;"]
