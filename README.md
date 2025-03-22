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
![image](https://github.com/user-attachments/assets/b931143a-f8ec-4df6-8a22-0002f544ab23)
![image](https://github.com/user-attachments/assets/cf30c03d-d950-4cfd-9b82-80e13aa29ba1)

**Product Service:**
![image](https://github.com/user-attachments/assets/5414a704-66aa-4f22-be5c-4acdd8a0a474)
![image](https://github.com/user-attachments/assets/869351e0-6d84-4f25-ba33-9f18ba5bc604)


**Order Service:**
![image](https://github.com/user-attachments/assets/21ec6ab4-08df-4018-8f6a-d609fa919692)
![image](https://github.com/user-attachments/assets/77d7ad57-bfc3-4781-833f-3aa45727299f)


**Gateway Service:**
![image](https://github.com/user-attachments/assets/850f0e4b-f1d0-49a7-98e5-1c4a6a0f2ddb)
![image](https://github.com/user-attachments/assets/31de87e3-5382-4ecf-b9ea-51c9259673ea)


## Containerization

### Building Docker Images

Build Docker images for each service:

```bash
docker build -t aakashrawat1910/microservices-user-service:latest ./user-service
docker build -t aakashrawat1910/microservices-product-service:latest ./product-service
docker build -t aakashrawat1910/microservices-order-service:latest ./order-service
docker build -t aakashrawat1910/microservices-gateway-service:latest ./gateway-service
```

![image](https://github.com/user-attachments/assets/8ed4f2b5-c757-4471-8d8a-86c3239a416f)


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

![image](https://github.com/user-attachments/assets/2c2a4dd9-d6eb-48ff-96d0-bee3d1a3a082)


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

![image](https://github.com/user-attachments/assets/caf6c1a4-2887-4aed-8fc6-eee1af5e9122)


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

![image](https://github.com/user-attachments/assets/bf4b37fb-040a-42e5-ad57-97ba8892f6bd)
![image](https://github.com/user-attachments/assets/587de661-470e-4dd4-8023-71dfb909349d)


### Deploying the Application

1. Pull the Docker images:

```bash
sudo docker pull aakashrawat1910/microservices-user-service:latest
sudo docker pull aakashrawat1910/microservices-product-service:latest
sudo docker pull aakashrawat1910/microservices-order-service:latest
sudo docker pull aakashrawat1910/microservices-gateway-service:latest
```

![image](https://github.com/user-attachments/assets/3054c4e6-506f-4ad0-943d-d57f364c33c0)


2. Create a `docker-compose.yml` file:

```bash
nano docker-compose.yml
```

Paste the Docker Compose configuration and save.

3. Start the application:

```bash
sudo docker-compose up -d
```

![image](https://github.com/user-attachments/assets/f73c66ef-58fe-4d4f-944f-6b0ac609ea8d)


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
![image](https://github.com/user-attachments/assets/a7365070-62d3-47f7-a3e4-e2129b618418)


**Product Service Endpoint:**
![image](https://github.com/user-attachments/assets/37b59cc8-997a-442a-bd23-f2685d4c74de)


**Order Service Endpoint:**
![image](https://github.com/user-attachments/assets/ca0f1b9e-e386-4000-8473-7599752856b1)


**Gateway Service Endpoint:**
![image](https://github.com/user-attachments/assets/497a2b12-5c7b-4bbf-bb86-2cdaa8a2ad99)


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
