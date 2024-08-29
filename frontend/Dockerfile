# Use a specific Node.js version for better consistency
FROM node:20

# Set working directory
WORKDIR /app

# Copy everything
COPY . .

# Install dependencies
RUN npm install

RUN npm install -g vite@^5.0.13

RUN npm run build

# Expose the port as in vite.config.ts file
EXPOSE 5173

# Set the working director to the dist artifact
WORKDIR /app/dist

# Start the Vite server
CMD ["npx", "serve", "-s", "-l", "5173", "-H", "0.0.0.0"]
#CMD ["npm", "run", "dev"]
