# Dockerfile for FastAPI Backend
FROM python:3.10-slim

# Install Poetry
RUN pip install poetry

WORKDIR /app

COPY . .

RUN poetry install
