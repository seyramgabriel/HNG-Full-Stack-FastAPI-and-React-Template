# Use a specific Node.js version for better consistency
FROM node:20

# Set working directory
WORKDIR /app

# Copy everything
COPY . .

# Install dependencies
RUN npm install

RUN npm install -g vite@^5.0.1.3

# Expose the port as in vite.config.ts file
EXPOSE 5173

# Start the Vite server if you don't start it via the docker-compose file
#CMD ["npm", "run", "dev"]
