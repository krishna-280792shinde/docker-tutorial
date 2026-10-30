# Docker – Flask Backend and Express Frontend

## 1. Project Overview

This project demonstrates how to containerize a Flask backend and an Express frontend using Docker and Docker Compose.

The objective is to build Docker images for both applications, run them as separate containers, configure communication between the containers, publish the images to Docker Hub, and maintain the source code in GitHub.

Docker allows applications to run in isolated containers with their required dependencies, making them easier to build, test, and deploy consistently.

## 2. Technologies Used

- Docker
- Docker Compose
- Python
- Flask
- Node.js
- Express.js
- Git and GitHub
- Docker Hub

## 3. Project Structure

The project follows this general structure:

```text
docker-tutorial/
├── backend/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
├── docker-compose.yml
├── .dockerignore
├── .gitignore
└── README.md
```

Note: Keep all additional source files, templates, configuration files, and dependencies required by your applications.

## 4. Application Architecture

The project consists of two services:

**Flask Backend**
- Implements the backend application using Python and Flask.
- Runs inside a Docker container.
- Exposes port `5000`.

**Express Frontend**
- Implements the frontend application using Node.js and Express.
- Runs inside a separate Docker container.
- Exposes port `3000`.
- Communicates with the backend through Docker Compose networking.

The frontend uses the backend service address:

`http://backend:5000`

Here, `backend` is the Docker Compose service name. It allows the frontend container to communicate with the backend container using Docker's internal network.

## 5. Prerequisites

Install the following tools before running the project:

- Docker Desktop
- Git
- A GitHub account
- A Docker Hub account
- Python and Node.js for optional local testing

Verify Docker installation:

```bash
docker --version
docker compose version
```

Verify Git installation:

```bash
git --version
```

## 6. Clone the Repository

Clone the project repository:

```bash
git clone <YOUR_DOCKER_GITHUB_REPOSITORY_URL>
cd Docker_KrishnaShinde
```

Replace the placeholder with your actual GitHub repository URL.

## 7. Build the Docker Images

The project uses separate Dockerfiles for the Flask backend and Express frontend.

Build both images using Docker Compose:

```bash
docker compose build
```

The example image names used in this project are:

- Backend: `krishna-backend:1.0`
- Frontend: `krishna-frontend:1.0`

Verify the images:

```bash
docker images
```

If the build fails, check that the Dockerfiles exist at the paths specified by the `build` configuration in `docker-compose.yml`.

## 8. Run the Applications Using Docker Compose

Start both services:

```bash
docker compose up -d
```

Check the running containers:

```bash
docker compose ps
```

View the application logs:

```bash
docker compose logs
```

View logs for an individual service:

```bash
docker compose logs backend
docker compose logs frontend
```

The applications are configured to expose these ports:

| Application | Container Port | Host Port |
|---|---:|---:|
| Flask Backend | 5000 | 5000 |
| Express Frontend | 3000 | 3000 |

## 9. Verify the Applications

Open the frontend in your browser:

http://localhost:3000/

If your Flask application provides a health endpoint, test it using:

```bash
curl http://localhost:5000/health
```

Expected response, if implemented:

```json
{"status":"healthy"}
```

You can also check the backend using the route implemented in your Flask application.

If the frontend needs to call the backend, verify that its configuration uses `http://backend:5000` from inside the Docker network, rather than `localhost:5000`.

## 10. Push Images to Docker Hub

Log in to Docker Hub:

```bash
docker login
```

Replace `<DOCKERHUB_USERNAME>` with your Docker Hub username.

Tag the backend image:

```bash
docker tag krishna-backend:1.0 <DOCKERHUB_USERNAME>/krishna-backend:1.0
```

Push the backend image:

```bash
docker push <DOCKERHUB_USERNAME>/krishna-backend:1.0
```

Tag the frontend image:

```bash
docker tag krishna-frontend:1.0 <DOCKERHUB_USERNAME>/krishna-frontend:1.0
```

Push the frontend image:

```bash
docker push <DOCKERHUB_USERNAME>/krishna-frontend:1.0
```

Verify that both images appear in your Docker Hub repositories.

Note: If your local image names differ, replace them with the names shown by `docker images`. The image references in your Compose file should match the actual published images when using Docker Hub for deployment.

## 11. Stop and Clean Up the Containers

Stop the running services:

```bash
docker compose down
```

This stops and removes the containers and the Compose network. Named volumes are not removed by this command unless additional options are used.

To rebuild the images after making source-code changes:

```bash
docker compose up -d --build
```

## 12. GitHub Repository

Repository URL: https://github.com/krishna-280792shinde/docker-tutorial

## 13. Troubleshooting

### Dockerfile not found

Check that the Dockerfiles exist and that the build paths in `docker-compose.yml` point to the correct directories.

### Frontend cannot connect to backend

Verify that:
- Both containers are running.
- Both services belong to the same Compose network.
- The frontend uses `http://backend:5000` for container-to-container communication.
- The backend listens on an interface accessible from other containers.

### Port already in use

Check whether another application is using port `3000` or `5000`. Stop the conflicting application or change the host-side port mapping.

### Docker Hub push fails

Verify that you are logged in and that the image has been tagged with your Docker Hub username.

## 14. Conclusion

This project demonstrates application containerization using Docker, multi-container orchestration using Docker Compose, communication between a Flask backend and an Express frontend, and publishing container images to Docker Hub.

The assignment provides practical experience with container builds, image management, networking, application testing, and source-code version control.
