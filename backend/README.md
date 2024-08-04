# Backend - FastAPI with PostgreSQL

This directory contains the backend of the application built with FastAPI and a PostgreSQL database.

## Prerequisites

- Python 3.8 or higher
- Poetry (for dependency management)
- PostgreSQL (ensure the database server is running)

### Installing Poetry

To install Poetry, follow these steps:

```sh
curl -sSL https://install.python-poetry.org | python3 -
```

Add Poetry to your PATH (if not automatically added):

## Setup Instructions

1. **Navigate to the backend directory**:
    ```sh
    cd backend
    ```

2. **Install dependencies using Poetry**:
    ```sh
    poetry install
    ```

3. **Set up the database with the necessary tables**:
    ```sh
    poetry run bash ./prestart.sh
    ```

    Make sure to set up a postgres database with the details in the backend .env file. Postgres must be running before 
    you run this command.

4. **Run the backend server**:
    ```sh
    poetry run uvicorn app.main:app --reload
    ```

5. **Update configuration**:
   Ensure you update the necessary configurations in the `.env` file, particularly the database configuration.
   - Update BACKEND_CORS_ORIGINS to reflect your domain name or server IP or localhost, must be consistent with the VITE_API_URL in your frontend .env file
   - Update Postgress database name, password, and user to your choice, but the server must remain localhost if it isto run on same machine as both your frontend and backend.


## Containerisation

1.  **Dockerfile**

    Make sure you have the Dockerfile in the backend directory. 

  Dockerfile
```
# Use the official Python image with the desired version
FROM python:3.11-slim

# Install system dependencies and build tools
RUN apt-get update && \
    apt-get install -y curl build-essential && \
    rm -rf /var/lib/apt/lists/*

# Install Poetry
RUN pip install poetry

# Add Poetry to PATH
ENV PATH="/root/.local/bin:$PATH"

# Set the working directory in the container
WORKDIR /app

# Copy pyproject.toml and poetry.lock to the working directory
COPY pyproject.toml poetry.lock* /app/

# Install dependencies using Poetry
RUN poetry install --no-root --no-interaction --no-ansi

# Copy the rest of the application code to the container
COPY . /app

# Ensure the application directory is in the Python path
ENV PYTHONPATH=/app

# Ensure the prestart.sh script is executable
RUN chmod +x ./prestart.sh

# Expose the application port
EXPOSE 8000

# Command to run the backend server
CMD poetry run bash ./prestart.sh;poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000
```

2. **Setting up ports and proxy**:
    - Ensure that you are in the root directory that contains both frontend and backend.
    - Install inginx
    - Edit the sites-available file in the root directory of this repository to reflect your domain name and IP address
    - Make sure to use a registered domain name and to have records for all sub-domains
    - Copy the content of sites-available file to /etc/nginx/sites-available/your-prefered-name eg. seyramgabriel


    - Run
      ```
      sudo ln -s /etc/nginx/sites-available/seyramgabriel /etc/nginx/sites-enabled/
      ```

    - Run
      ```
      sudo service nginx restart
      ```

    - Create a docker-compose.yml file as in the root directory

    docker-compose.yml
```
version: '3.8'

services:

  postgres:
    image: postgres:13
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: changethis123
      POSTGRES_DB: app
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  backend:
    build: ./backend
    volumes:
      - ./backend:/app
    ports:
      - "8000:8000"
    depends_on:
      - postgres
    environment:
      - POSTGRES_SERVER=postgres
      - POSTGRES_PORT=5432
      - POSTGRES_DB=app
      - POSTGRES_USER=app
      - POSTGRES_PASSWORD=changethis123
      - BACKEND_CORS_ORIGINS=https://seyramgabriel.com,http://localhost,http://localhost:5173,https://localhost,https://localhost:5173
      - SECRET_KEY=changethis123
      - FIRST_SUPERUSER=devops@hng.tech
      - FIRST_SUPERUSER_PASSWORD=devops#HNG11
      - USERS_OPEN_REGISTRATION=True

  frontend:
    build: ./frontend
    command: ["npm", "run", "dev"]
    volumes:
      - ./frontend:/app
    ports:
      - "5173:5173"
  #   depends_on:
  #    - backend

  # nginx:
  #   build: .
  #   container_name: nginx
  #   ports:
  #     - "80:80"
  #     - "443:443"
  #   volumes:
  #     - ./nginx.conf:/etc/nginx/nginx.conf
  #     - /etc/letsencrypt:/etc/letsencrypt
  #     - /var/lib/letsencrypt:/var/lib/letsencrypt
  #   depends_on:
  #     - frontend
  #     - backend
  #     - proxy-manager
  #     - adminer

  # nginx:
  #   image: nginx:latest
#    volumes:
    #  - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    #  - ./nginx/ssl:/etc/nginx/ssl
    #  - ./nginx/nginx.conf:/etc/nginx/nginx.conf
    #  - ./nginx/letsencrypt:/etc/letsencrypt
    #  - ./nginx/ssl:/etc/nginx/ssl
    #  - ./nginx/html:/usr/share/nginx/html
  #  ports:
  #    - "80:80"
  #    - "443:443"
    # depends_on:
    #   - frontend
    #   - backend

  adminer:
    image: adminer:latest
    restart: always
    ports:
      - "8080:8080"
    depends_on:
      - postgres
    environment:
      ADMINER_DEFAULT_SERVER: postgres

  proxy-manager:
    image: jc21/nginx-proxy-manager:latest
    restart: unless-stopped
    ports:
#     - "81:81"
     - "8090:81" # For accessing NPM UI
#     - "80:80"
#     - "443:443"
    volumes:
      - ./proxy/data:/data
      - ./proxy/letsencrypt:/etc/letsencrypt
      - ./proxy/ssl:/etc/nginx/ssl
    depends_on:
      - nginx
      - adminer
volumes:
  postgres_data:
```

3. **Building and running docker images**
    - Run the following command to build the docker image:
     ```
     docker-compose up -d
     ```

4. **Run the following commands to check the images built and the containers created/running**

    ```docker images``` # Show images

    ```docker ps -a```  # Containers created

    ```docker ps```     # Containers running

5. **Accessing the application**

 You can now access the application on your domain names as below:
 
 ```seyramgabriel.com```    # Replace with your domain name

 ```www.seyramgabriel.com```

 ```db.seyramgabriel.com```

 ```www.seyramgabriel.com```

 ```proxy.seyramgabriel.com```

 ```www.proxy.seyramgabriel.com```


6. **SSL Certificates**

 Run this to install certbot
 ```
 sudo apt update
 sudo apt install certbot python3-certbot-nginx
 ```

 To obtain certificates for your domain names and get your nginx configured automatically, run the following. Replace domain names with your registered domain names.
 ```
 sudo certbot --nginx -d seyramgabriel.com -d www.seyramgabriel.com -d proxy.seyramgabriel.com -d www.proxy.seyramgabriel.com -d db.seyramgabriel.com -d www.db.seyramgabriel.com```

