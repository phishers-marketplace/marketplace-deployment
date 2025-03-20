
# Phishers Marketplace Deployment

This repository contains the deployment configuration for the Phishers Marketplace application, which includes both the frontend and API components.

## Prerequisites

- Docker and Docker Compose installed on your system
- SSL certificate and private key for HTTPS

## Setup Instructions

1. Clone this repository:
   ```bash
   git clone https://github.com/phishers-marketplace/marketplace-deployment.git
   cd marketplace-deployment
   ```

2. Initialize and update submodules:
   ```bash
   git submodule init
   git submodule update
   ```

3. Copy your SSL certificate and private key:
   ```bash
   mkdir -p nginx/ssl
   # Copy your certificate and private key to nginx/ssl/
   # - cert.pem: Your SSL certificate
   # - key.pem: Your SSL private key
   ```

4. Set up environment variables:
   - Copy the `.env.example` file from the API repository to `marketplace-api/.env`
   - Copy the `.env.example` file from the Frontend repository to `marketplace-frontend/.env`
   - Update the environment variables as needed

5. Start the application:
   ```bash
   docker-compose up -d
   ```

## Architecture

- Frontend: React application running on port 3000 (internal)
- API: Python FastAPI application running on port 9000 (internal)
- MongoDB: Database running on port 27017 (internal)
- Nginx: Reverse proxy handling HTTPS on port 443 (external)

All services are connected through an internal Docker network, and only the Nginx service is exposed to the host system.

## Security

- The API is not directly accessible from outside the Docker network
- All external traffic is handled through Nginx with HTTPS
- MongoDB is only accessible within the Docker network

## Maintenance

To update the application:

1. Pull the latest changes:
   ```bash
   git pull
   git submodule update --remote
   ```

2. Rebuild and restart the containers:
   ```bash
   docker-compose down
   docker-compose up -d --build
   ```

## Troubleshooting

1. Check container logs:
   ```bash
   docker-compose logs -f [service_name]
   ```

2. Access container shell:
   ```bash
   docker-compose exec [service_name] sh
   ```

3. Common issues:
   - If the SSL certificate is not loading, check the permissions and paths in nginx/ssl/
   - If services can't connect, ensure all environment variables are properly set
   - If MongoDB fails to start, check the mongo-init.js file and environment variables