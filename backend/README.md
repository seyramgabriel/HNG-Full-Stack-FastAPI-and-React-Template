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
   - Update BACKEND_CORS_ORIGINS to reflect your domain name or server IP
   - Update Postgress database name, password, and user to your choice, but the server must remain localhost if it is 
     to run on same machine as both your frontend and backend.


## Containerisation

1.  **Dockerfile**

    Make sure you have the Dockerfile in the backend directory. 

2. **root directory**:
    - cd back to the directory that contains both frontend and backend.
    - Install inginx
    - Edit the sites-available file in the root directory of this repository to reflect your domain name and IP address
    - Copy the content to /etc/nginx/sites-available/your-prefered name eg.seyramgabriel
    - Run ```
      sudo ln -s /etc/nginx/sites-available/seyramgabriel /etc/nginx/sites-enabled/
      ```
    - Run
      ```
      sudo service nginx restart
      ```
    - Create a docker-compose.yml file as in the root director.
    - Run the following command to build the docker image:
     ```
     docker-compose up -d
     ```
