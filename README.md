# FastAPI Docker Project

A modern, fast (high-performance), web framework for building APIs with Python 3.11+, containerized with Docker and automated with Jenkins.

## Features

- **FastAPI**: High performance, easy to learn, fast to code, ready for production.
- **Dockerized**: Easy deployment and environment consistency.
- **CI/CD Integrated**: Jenkins pipeline for automated builds and Docker Hub pushing.
- **Health Checks**: Built-in `/health` endpoint for monitoring.

## Tech Stack

- **Framework**: [FastAPI](https://fastapi.tiangolo.com/)
- **ASGI Server**: [Uvicorn](https://www.uvicorn.org/)
- **Containerization**: [Docker](https://www.docker.com/)
- **Automation**: [Jenkins](https://www.jenkins.io/)

## Local Development

### Prerequisites

- Python 3.11+
- Docker (optional, for containerized running)

### Running Locally (Without Docker)

1. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

2. **Run the application**:
   ```bash
   uvicorn app.main:app --reload
   ```

3. **Ineractive API Docs**:
   - Swagger UI: [http://localhost:8000/docs](http://localhost:8000/docs)
   - ReDoc: [http://localhost:8000/redoc](http://localhost:8000/redoc)

### Running with Docker

1. **Build the image**:
   ```bash
   docker build -t fastapi-docker-app .
   ```

2. **Run the container**:
   ```bash
   docker run -d -p 8000:8000 fastapi-docker-app
   ```

## CI/CD Pipeline

The project includes a `Jenkinsfile` that defines a declarative pipeline with the following stages:

1. **Checkout**: Pulls code from source control.
2. **Build Docker Image**: Builds the Docker image from the `Dockerfile`.
3. **Login to Docker Hub**: Authenticats with Docker Hub using Jenkins credentials.
4. **Tag and Push Image**: Tags the image with `latest` and the build number, then pushes to `kennyjaymes/fastapi-docker-project`.

## API Endpoints

- `GET /`: Returns a simple "Hello World" message.
- `GET /health`: Returns the health status of the application.
