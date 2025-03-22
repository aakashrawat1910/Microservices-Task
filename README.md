# Microservices Application

A containerized microservices application built with Node.js, Docker, and Docker Compose.

## Overview

This project consists of four independent microservices:

- **User Service** (Port 3000): Manages user data
- **Product Service** (Port 3001): Handles product information
- **Order Service** (Port 3002): Manages order processing
- **Gateway Service** (Port 3003): API gateway for routing requests

## Table of Contents

- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Local Development](#local-development)
- [Containerization](#containerization)
- [Deployment on EC2](#deployment-on-ec2)
- [Testing](#testing)
- [Troubleshooting](#troubleshooting)

## Prerequisites

- Node.js (v14 or higher)
- Docker
- Docker Compose
- Git

## Project Structure

```
├── user-service/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
├── product-service/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
├── order-service/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
├── gateway-service/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
├── docker-compose.yml
└── README.md
```

## Local Development

### Running Services Locally

To run each service locally:

1. Navigate to each service directory
2. Install dependencies:
   ```
   npm install
   ```
3. Start the service:
   ```
   node app.js
   ```

**User Service:**
![User Service Running Locally](path/to/user-service-local-screenshot.png)

**Product Service:**
![Product Service Running Locally](path/to/product-service-local-screenshot.png)

**Order Service:**
![Order Service Running Locally](path/to/order-service-local-screenshot.png)

**Gateway Service:**
![Gateway Service Running Locally](path/to/gateway-service-local-screenshot.png)

## Containerization

### Building Docker Images

Build Docker images for each service:

```bash
docker build -t aakashrawat1910/microservices-user-service:latest ./user-service
docker build -t aakashrawat1910/microservices-product-service:latest ./product-service
docker build -t aakashrawat1910/microservices-order-service:latest ./order-service
docker build -t aakashrawat1910/microservices-gateway-service:latest ./gateway-service
```

![Building Docker Images](path/to/build-images-screenshot.png)

### Pushing Images to Docker Hub

Make sure to authenticate with Docker Hub:

```bash
docker login
```

Then push the images:

```bash
docker push aakashrawat1910/microservices-user-service:latest
docker push aakashrawat1910/microservices-product-service:latest
docker push aakashrawat1910/microservices-order-service:latest
docker push aakashrawat1910/microservices-gateway-service:latest
```

![Pushing to Docker Hub](path/to/docker-push-screenshot.png)

### Docker Compose

The `docker-compose.yml` file is configured to run all four services together with proper networking and port mappings.

To start the application using Docker Compose:

```bash
docker-compose up -d
```

This will start all services in detached mode.

To stop the application:

```bash
docker-compose down
```

## Deployment on EC2

### Setting up EC2 Instance

1. Launch an EC2 instance with Ubuntu
2. Configure security groups to allow inbound traffic on ports 3000-3003
3. Connect to your instance using SSH

![EC2 Instance Creation](path/to/ec2-creation-screenshot.png)

### Installing Docker and Docker Compose

```bash
# Update package lists
sudo apt update

# Install Docker
sudo apt install docker.io -y

# Start and enable Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Add current user to docker group (optional)
sudo usermod -aG docker $USER

# Install Docker Compose
sudo apt install docker-compose -y
```

![Docker Installation](path/to/docker-installation-screenshot.png)

### Deploying the Application

1. Pull the Docker images:

```bash
sudo docker pull aakashrawat1910/microservices-user-service:latest
sudo docker pull aakashrawat1910/microservices-product-service:latest
sudo docker pull aakashrawat1910/microservices-order-service:latest
sudo docker pull aakashrawat1910/microservices-gateway-service:latest
```

![Pulling Docker Images](path/to/docker-pull-screenshot.png)

2. Create a `docker-compose.yml` file:

```bash
nano docker-compose.yml
```

Paste the Docker Compose configuration and save.

3. Start the application:

```bash
sudo docker-compose up -d
```

![Docker Compose Up](path/to/docker-compose-up-screenshot.png)

## Testing

### Testing Endpoints

You can test the services using curl or a web browser:

```bash
curl http://localhost:3000/api/users
curl http://localhost:3001/api/products
curl http://localhost:3002/api/orders
curl http://localhost:3003/api/gateway
```

Or replace `localhost` with your EC2 instance's public IP:

```bash
curl http://<your-ec2-public-ip>:3000/api/users
```

**User Service Endpoint:**
![User Service Test](path/to/user-service-test-screenshot.png)

**Product Service Endpoint:**
![Product Service Test](path/to/product-service-test-screenshot.png)

**Order Service Endpoint:**
![Order Service Test](path/to/order-service-test-screenshot.png)

**Gateway Service Endpoint:**
![Gateway Service Test](path/to/gateway-service-test-screenshot.png)

## Troubleshooting

### Common Issues and Solutions

1. **Services cannot connect to each other:**
   - Ensure all services are running (`docker ps`)
   - Check if the Docker network is properly configured
   - Verify service names match the ones in Docker Compose file

2. **Port conflicts:**
   - Check if ports 3000-3003 are already in use
   - Run `sudo netstat -tulpn | grep <port>` to identify processes
   - Either stop the conflicting process or change the port mapping in docker-compose.yml

3. **Docker images not working:**
   - Try rebuilding the images locally
   - Check Dockerfile for errors
   - Ensure package.json and dependencies are correctly configured

4. **EC2 connection issues:**
   - Verify security group settings allow traffic on required ports
   - Check if the instance is running
   - Confirm you are using the correct public IP address

### Logs

To check the logs for a specific service:

```bash
docker-compose logs user-service
docker-compose logs product-service
docker-compose logs order-service
docker-compose logs gateway-service
```

For real-time logs:

```bash
docker-compose logs -f service-name
```

---

## Repository

This project is available on GitHub:
[https://github.com/aakashrawat1910/Microservices-Task](https://github.com/aakashrawat1910/Microservices-Task)