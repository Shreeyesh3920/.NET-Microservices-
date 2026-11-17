<img width="958" height="508" alt="image" src="https://github.com/user-attachments/assets/9766c029-3857-42c1-8cf2-07afabb041e6" />

## 1. What is Azure Container Registry?
Azure Container Registry is a private registry where you store and manage your Docker/OCI container images.

Think of it like GitHub, but instead of storing your source code, it stores your Docker images.

For example:
- GitHub stores your .NET source code.
- Docker builds your source code into a Docker image.
- Azure Container Registry stores that Docker image.
- Azure Container Apps or AKS pulls that image and runs it as a container.

<img width="993" height="623" alt="image" src="https://github.com/user-attachments/assets/6ff36ce5-c9cf-4960-ac3b-433e58b34ee2" />

## 2. Let's connect it to your Docker knowledge
Remember your local Docker workflow?

You have three microservices:
Microservice	Docker image
Product Service	product-service:v1
Order Service	order-service:v1
User Service	user-service:v1

When you run docker build, Docker creates an image.

When you run docker run, Docker creates and starts a container from that image.

ACR comes into the picture when you want to store those images in Azure instead of keeping them only on your laptop.

