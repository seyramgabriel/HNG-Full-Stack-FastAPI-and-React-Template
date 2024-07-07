# Stage 1: Build the React application
FROM node:16 as build

WORKDIR /app

# Copy package.json and package-lock.json
COPY package.json package-lock.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run build

# Debugging step: List the contents of the build directory
RUN ls -al /app/dist

# Stage 2: Serve the React application with Nginx
FROM nginx:latest

# Copy the built React application from the previous stage
COPY --from=build /app/dist /usr/share/nginx/html

# Copy the Nginx configuration file
COPY app  /etc/nginx/sites-available/app

# Expose port 80
EXPOSE 80

# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]

