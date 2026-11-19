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



# Azure Container Registry (ACR)

## 2. Create ACR in Azure Portal

1. Search for **Container Registries** in Azure Portal.
2. Click **Create**.
3. Select Subscription and Resource Group.
4. Enter Registry Name.
5. Select Region and SKU (Basic is suitable for learning).
6. Click **Review + Create**.

After creation, open ACR → Overview and copy the **Login server**.

Example:

```text
Registry Name: mydemoregistry
Login Server: mydemoregistry.azurecr.io
```

## 3. Login to Azure

Install Azure CLI and execute:

```bash
az login
```

## 4. Login to ACR

```bash
az acr login --name mydemoregistry
```

This authenticates Docker with your Azure Container Registry.

## 5. Build Docker Image

Run from the solution root directory:

```bash
docker build -t demowebapplication:v1 -f DemoWebApplication/Dockerfile .
```

- `-t`: Assign image name and tag.
- `-f`: Specify Dockerfile location.
- `.`: Build context (current directory).

Check images:

```bash
docker images
```

## 6. Tag Docker Image

Tag the local image with the ACR login server:

```bash
docker tag demowebapplication:v1 mydemoregistry.azurecr.io/demowebapplication:v1
```

Format:

```text
ACR_LOGIN_SERVER/IMAGE_NAME:TAG
```

## 7. Push Image to ACR

```bash
docker push mydemoregistry.azurecr.io/demowebapplication:v1
```

Docker uploads the image to Azure Container Registry.

## 8. Verify Image

Azure Portal → Container Registry → Repositories.

You should see:

```text
demowebapplication
    └── v1
```

## Complete Command Summary

```bash
# Login to Azure
az login

# Login to ACR
az acr login --name mydemoregistry

# Build Docker image
docker build -t demowebapplication:v1 -f DemoWebApplication/Dockerfile .

# Tag image
docker tag demowebapplication:v1 mydemoregistry.azurecr.io/demowebapplication:v1

# Push image to ACR
docker push mydemoregistry.azurecr.io/demowebapplication:v1
```

**Note:** Replace `mydemoregistry` with your actual ACR name.

## Important Concepts

| Command | Purpose |
|---|---|
| `docker build` | Creates Docker image |
| `docker images` | Lists local images |
| `docker tag` | Tags image for ACR |
| `az acr login` | Authenticates with ACR |
| `docker push` | Uploads image to ACR |

**Remember:**

- Docker builds and manages container images.
- ACR stores container images.
- ACR does not run containers.
- Azure App Service can pull an image from ACR and run it.

