# Microservices Application

This repository contains a containerized microservices architecture built with Node.js, Docker, and Docker Compose. The application consists of four interconnected services: User, Product, Order, and a Gateway service that acts as an API gateway.

## Architecture Overview

The application is structured as follows:

- **User Service** (Port 3000): Manages user data and authentication
- **Product Service** (Port 3001): Handles product catalog and inventory
- **Order Service** (Port 3002): Processes customer orders
- **Gateway Service** (Port 3003): Acts as an API gateway, routing requests to appropriate services

## Prerequisites

- Docker and Docker Compose installed on your system
- Docker Hub account for image storage
- Git (for cloning the repository)
- AWS account (for EC2 deployment)

## Building and Deploying

### 1. Building Docker Images Locally

Navigate to your project directory and build images for each service:

```bash
# Build User Service
docker build -t yourusername/microservices-user-service:latest ./user-service

# Build Product Service
docker build -t yourusername/microservices-product-service:latest ./product-service

# Build Order Service
docker build -t yourusername/microservices-order-service:latest ./order-service

# Build Gateway Service
docker build -t yourusername/microservices-gateway-service:latest ./gateway-service
```

Replace `yourusername` with your Docker Hub username.

### 2. Pushing Images to Docker Hub

First, log in to Docker Hub:

```bash
docker login
# Enter your Docker Hub credentials when prompted
```

Then push your images:

```bash
docker push yourusername/microservices-user-service:latest
docker push yourusername/microservices-product-service:latest
docker push yourusername/microservices-order-service:latest
docker push yourusername/microservices-gateway-service:latest
```

### 3. Deploying on EC2

For detailed instructions on deploying to AWS EC2 using Docker Hub images, see the [EC2 Deployment Guide](./EC2_DEPLOYMENT.md).

### 4. Running Locally with Docker Compose

For local testing using Docker Hub images:

```bash
docker-compose up
```

To run in detached mode:
```bash
docker-compose up -d
```

To stop all services:
```bash
docker-compose down
```

## Testing the Services

Once all services are up and running, you can test them using the following endpoints:

### Direct Service Access

#### User Service
```bash
curl http://localhost:3000/users
```
Or open in browser: [http://localhost:3000/users](http://localhost:3000/users)

#### Product Service
```bash
curl http://localhost:3001/products
```
Or open in browser: [http://localhost:3001/products](http://localhost:3001/products)

#### Order Service
```bash
curl http://localhost:3002/orders
```
Or open in browser: [http://localhost:3002/orders](http://localhost:3002/orders)

### Via Gateway Service

#### Users API
```bash
curl http://localhost:3003/api/users
```

#### Products API
```bash
curl http://localhost:3003/api/products
```

#### Orders API
```bash
curl http://localhost:3003/api/orders
```

## Troubleshooting

### Container Issues

If a service is not responding:

1. Check container status:
```bash
docker-compose ps
```

2. View logs for a specific service:
```bash
docker-compose logs user-service
```
Replace `user-service` with the appropriate service name.

3. Restart a specific service:
```bash
docker-compose restart user-service
```

### Network Issues

1. Ensure all containers are on the same network:
```bash
docker network inspect microservices-network
```

2. Verify that the service is listening on the correct port inside the container:
```bash
docker exec -it user-service netstat -tulpn
```

### Docker Hub Issues

1. If you're unable to push to Docker Hub, verify your credentials:
```bash
docker login
```

2. Check image names match your Docker Hub repository:
```bash
docker images
```

## Screenshots

### Services Running
![Services Running](./screenshots/services-running.png)

### User Service Response
![User Service](./screenshots/user-service.png)

### Product Service Response
![Product Service](./screenshots/product-service.png)

### Order Service Response
![Order Service](./screenshots/order-service.png)

### Gateway Service Response
![Gateway Service](./screenshots/gateway-service.png)

## Project Structure

```
microservices-task/
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
├── deploy.sh
└── README.md
```