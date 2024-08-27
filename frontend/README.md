# Frontend - ReactJS with ChakraUI

This directory contains the frontend of the application built with ReactJS and ChakraUI.

## Prerequisites

- Node.js (version 14.x or higher)
- npm (version 6.x or higher)

## Manual Setup Instructions:

1. **Navigate to the frontend directory**:
    ```sh
    cd frontend
    ```

2. **Install dependencies**:
    ```sh
    npm install
    ```

3. **Run the development server**:
    ```sh
    npm run dev
    ```

4. **Configure API URL**:

   Ensure the API URL is correctly set in the `.env` file.
   
   Use ```https://seyramgabriel.com``` if you have your dns properly registered and configured.

   Use ```http://localhost:5173``` if you are running on a localhost

   Use ```http://IP-address:5173``` if you are running on a virtual machine

## Containerisation:
You can run the install.sh script to install docker and nginx 

1. **Dockerfile**:
    Make sure you have the Dockerfile in the frontend directory.

    Dockerfile
```
# Use a specific Node.js version for better consistency
FROM node:20

# Set working directory
WORKDIR /app

# Copy everything
COPY . .

# Install dependencies
RUN npm install

# Expose the port as in vite.config.ts file
EXPOSE 5173

# Start the Vite server
# CMD ["npm", "run", "dev"]
```

2. **Frontend .env**
   Edit the frontend/.env file to reflect your domain name or IP address

3. **Next steps**: Proceed to backend README.md file for continuation of containerisation. 



   
