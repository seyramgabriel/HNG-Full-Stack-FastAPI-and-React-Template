# Dockerfile for FastAPI Backend
FROM python:3.10-slim

# Install Poetry
RUN pip install poetry

WORKDIR /app

# Copy poetry files
COPY pyproject.toml poetry.lock ./

# Install dependencies
RUN poetry config virtualenvs.create false && poetry install --no-dev

COPY . .

EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
