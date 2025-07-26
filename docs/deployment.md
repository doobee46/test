# Deployment

> Part of the [test documentation](README.md)

# Deployment Guide

This deployment guide provides comprehensive instructions for deploying your application to production environments, covering prerequisites, environment setup, build processes, deployment options, and more.

## 1. Deployment Prerequisites

Before deploying the application, ensure you have the following prerequisites installed and configured:

- **Node.js**: Version 14.x or higher
- **npm**: Version 6.x or higher
- **Docker**: Installed for container deployment
- **Kubernetes**: Access to a Kubernetes cluster for orchestration
- **Cloud Provider Account**: AWS, GCP, or Azure account set up
- **Git**: Version control system to manage your codebase
- **Infrastructure as Code Tool**: Terraform or similar, if applicable

## 2. Environment Setup

Set up the production environment by following these steps:

### On a Local Machine:

1. Clone the repository:
   ```bash
   git clone https://github.com/your-repo.git
   cd your-repo
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file in the root directory to configure the environment variables:
   ```bash
   touch .env
   ```

### Sample `.env` Configuration:
```plaintext
NODE_ENV=production
DATABASE_URL=your_database_url
API_KEY=your_api_key
```

### On a Cloud Provider:

Follow the documentation specific to your cloud provider (AWS, GCP, Azure) to set up the required resources (VMs, databases, etc.).

## 3. Build Process

To prepare the application for deployment, you need to build it:

1. Run the build command:
   ```bash
   npm run build
   ```

2. Confirm that the build was successful by checking the output in the `dist` directory (or other designated build output).

## 4. Deployment Steps for Different Platforms

### A. Deploying to AWS EC2

1. **Upload the Build**:
   ```bash
   scp -i your_key.pem -r ./dist ec2-user@your-ec2-instance:/var/www/your-app
   ```

2. **SSH into your Instance**:
   ```bash
   ssh -i your_key.pem ec2-user@your-ec2-instance
   ```

3. **Start the Application**:
   ```bash
   cd /var/www/your-app
   npm install --production
   npm start
   ```

### B. Deploying with Docker

1. **Build the Docker Image**:
   ```bash
   docker build -t your-app:latest .
   ```

2. **Run the Container**:
   ```bash
   docker run -d -p 80:3000 --env-file .env your-app:latest
   ```

### C. Deploying to Kubernetes

1. **Create a Deployment YAML**:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: your-app
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: your-app
     template:
       metadata:
         labels:
           app: your-app
       spec:
         containers:
         - name: your-app
           image: your-app:latest
           ports:
           - containerPort: 3000
           envFrom:
           - configMap: your-config
   ```

2. **Apply the Deployment**:
   ```bash
   kubectl apply -f deployment.yaml
   ```

## 5. Production Configuration

Make sure to define the production configuration within your application.

### Sample Configuration:
```json
{
  "database": {
    "url": "your_database_url"
  },
  "api": {
    "key": "your_api_key"
  }
}
```

## 6. Health Checks and Monitoring

Set up health checks to ensure the application is running smoothly.

### For AWS EC2:
- Use an Application Load Balancer with health check settings pointing to your health endpoint.

### For Kubernetes:
- Define a health check in your deployment YAML:
```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 3000
  initialDelaySeconds: 30
  periodSeconds: 10
```

## 7. CI/CD Pipeline Setup

Set up a CI/CD pipeline using platforms like GitHub Actions, GitLab CI, or Jenkins.

### Example GitHub Actions Configuration:
```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'
      - name: Install dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        run: ./deploy.sh
```

## 8. Rollback Procedures

If you encounter issues after deployment, follow these rollback steps:

### For Docker:
1. Stop the running container:
   ```bash
   docker stop <container_id>
   ```

2. Remove the container:
   ```bash
   docker rm <container_id>
   ```

3. Rollback to the previous image:
   ```bash
   docker run -d -p 80:3000 your-app:previous_version
   ```

### For Kubernetes:
1. Rollback the deployment:
   ```bash
   kubectl rollout undo deployment/your-app
   ```

By following this guide, you should be able to successfully deploy your application to a production environment, with options for rollback and ongoing monitoring.