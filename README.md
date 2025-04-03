# Deploying a Static HTML Website on Kubernetes using Docker & Nginx

## Project Overview
This project demonstrates how to deploy a **static HTML website** on **Kubernetes** using **Docker** and **Nginx**. The project involves:
- **Containerizing a simple HTML website** using Docker
- **Pushing the Docker image** to Docker Hub
- **Deploying the application** on Kubernetes
- **Exposing the application** using a Kubernetes **Service**
- **Implementing Rolling Updates** for seamless version updates

## Tech Stack
- **Docker** - Containerization of the web application
- **Nginx** - Web server for serving static files
- **Kubernetes** - Orchestration and deployment
- **Minikube** - Local Kubernetes cluster for testing
- **Docker Hub** - Storing and sharing Docker images

## Steps to Run the Project

### 1. Build and Run with Docker

#### Build the Docker Image:
```bash
docker build -t kiran22222/nginx-kiran .
```

#### Run the Container Locally:
```bash
docker run -d -p 80:80 kiran22222/nginx-kiran
```

🔹 Open `http://localhost` in a browser to check the website.

#### Push Image to Docker Hub:
```bash
docker login
docker push kiran22222/nginx-kiran
```

### 2. Deploying on Kubernetes

#### Create a Deployment File (`deployment.yaml`)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-kiran
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx-kiran
  template:
    metadata:
      labels:
        app: nginx-kiran
    spec:
      containers:
      - name: nginx-kiran
        image: kiran22222/nginx-kiran
        ports:
        - containerPort: 80
```

#### Create a Service File (`service.yaml`)
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-kiran-service
spec:
  selector:
    app: nginx-kiran
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

#### Apply Kubernetes Manifests:
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

#### Check Deployment & Service Status:
```bash
kubectl get pods
kubectl get services
```

#### Access the Website:
```bash
minikube service nginx-kiran-service --url
```

### 3. Implementing Rolling Updates

#### Update Deployment to a New Version
If you make changes and rebuild your Docker image, push it to Docker Hub as a new version:
```bash
docker build -t kiran22222/nginx-kiran:v2 .
docker push kiran22222/nginx-kiran:v2
```

#### Update Kubernetes Deployment with New Image
```bash
kubectl set image deployment/nginx-kiran nginx-kiran=kiran22222/nginx-kiran:v2
```

#### Check the Rolling Update Status
```bash
kubectl rollout status deployment/nginx-kiran
```

#### Rollback if Needed
```bash
kubectl rollout undo deployment/nginx-kiran
```

## Key Features
- **Containerized Nginx web server** for hosting a static website
- **Scalable Kubernetes Deployment** using replicas
- **Rolling Updates** for seamless application updates
- **Exposed via Kubernetes Service** for easy access
- **Minikube support** for local Kubernetes testing


## Future Enhancements
- **Ingress Controller** for better routing
- **Persistent Storage** for enhanced data management
- **CI/CD Pipeline** to automate deployments
- **Load Balancing** for better traffic distribution

## Contact & Resources
🔹 **GitHub:** [Kiran's Profile](https://github.com/Kiran22222)
🔹 **Docker Hub:** [Kiran's Docker Hub](https://hub.docker.com/u/kiran22222)

🎉 **Happy Deploying!** 🚀

