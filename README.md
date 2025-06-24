# Microservices Application with Nginx Reverse Proxy

## Overview
This project demonstrates a basic microservices architecture using Docker and Nginx. It consists of two light weight services built in Golang and Python, each exposing simple HTTP end points. These services are containerized and served behind an Nginx reverse proxy, with all services orchestrated via Docker Compose.

## Project Structure

.
├── docker-compose.yml
├── nginx/
│   └── nginx.conf
├── service_1/             # Golang service
│   ├── Dockerfile
│   ├── main.go
│   └── go.mod
└── service_2/             # Python Flask service
    ├── Dockerfile
    └── app.py

## Services

  * Service 1 – Golang
- Listens on: 8001
- Endpoints:
  - GET /ping → Health check
  - GET /hello → Returns greeting from service
- Healthcheck: Configured in Docker Compose

  * Service 2 – Python (Flask)
- Listens on: 8002
- Endpoints:
  - GET /ping → Health check
  - GET /hello → Returns greeting from service
- Healthcheck: Configured in Docker Compose
  
  * Nginx – Reverse Proxy
- Listens on: 8080
- Routes:
  - /service1/* → Proxies to Service 1
  - /service2/* → Proxies to Service 2

    
## Sample Requests

* Service 1 (Golang)
curl http://localhost:8080/service1/ping
curl http://localhost:8080/service1/hello

* Service 2 (Python)
curl http://localhost:8080/service2/ping
curl http://localhost:8080/service2/hello


## How to Run

* Prerequisites
- Docker
- Docker Compose
  
* Steps to Run

1. Build and start the entire stack:
  
docker-compose up --build

2. To stop:
  
docker-compose down

3. Health Checks

Both microservices include Docker health checks to ensure they are responsive before Nginx forwards traffic to them.
healthcheck:
  test: ["CMD", "curl", "-f", "http://localhost:<port>/ping"]
  interval: 10s
  timeout: 5s
  retries: 3
  
## Networking
A custom bridge network `app-net` ensures all services can communicate internally. Nginx uses this internal network to forward traffic to the services.

Summary

- 2 microservices (Golang + Python) built and containerized
- Nginx reverse proxy setup with route prefixes
- Docker Compose orchestrates build, deployment, and networking
- Health checks for production-readiness
- Verified with curl HTTP requests
  
## Future Enhancements

- Add logging via Fluent Bit or Loki
- Include Prometheus/Grafana monitoring
- Add frontend (React/HTML) served through Nginx
- CI/CD integration using GitHub Actions or Jenkins
  
## Author
   Parnita Bokade
