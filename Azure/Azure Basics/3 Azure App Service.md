# Deploy Docker Image to Azure App Service

## 1. What is Azure App Service?

Azure App Service is a fully managed **Platform as a Service (PaaS)** offering from Microsoft Azure that allows us to host web applications and APIs without managing the underlying infrastructure.

For example, suppose we have developed an ASP.NET Core Web API. We need a hosting environment to run our application and make it accessible over the internet.

Azure App Service provides that environment.

### Key Responsibilities

- Application hosting.
- Server infrastructure management.
- HTTPS support.
- Scaling.
- Application monitoring and logging.

### What can we deploy?

- ASP.NET Core applications.
- Java applications.
- Custom Docker containers.

**Important:** We are using Azure App Service with a Linux Custom Container to host our Dockerized .NET application.

---

## 2. App Service vs App Service Plan

While creating an App Service, Azure also requires an App Service Plan.

### What is an App Service Plan?

An App Service Plan defines the underlying compute resources on which our web applications run.

It determines:

- CPU and memory resources.
- Number of instances.
- Scaling capabilities.
- Pricing tier.
- Available hosting features.

| App Service | App Service Plan |
|---|---|
| Hosts our web application | Provides compute resources |
| Provides the application URL | Defines pricing and capacity |
| Runs our application | Determines available compute resources |

**Important:** Multiple App Services can share the same App Service Plan.

---

## 3. Create App Service in Azure Portal

1. Open Azure Portal.
2. Search for **App Services**.
3. Click **Create → Web App**.
4. Configure the following properties.

### Basic Configuration

| Property | Value |
|---|---|
| Subscription | Your Azure Subscription |
| Resource Group | Demo-Resource-Group |
| Name | Demo-Service-Plan |
| Publish | Container |
| Operating System | Linux |
| Region | West US 3 |
| Pricing Plan | Basic (B1) |

### Important Properties

**Publish: Container**

We select Container because we want Azure App Service to run our Docker image instead of deploying application source code directly.

**Operating System: Linux**

We select Linux because our Docker image uses the Linux-based .NET runtime image.

**Pricing Plan: Basic (B1)**

We use Basic (B1) for our custom-container hosting setup.

Click **Review + Create → Create**.

---

## 4. Configure ACR Image in App Service

We already have our Docker image stored in Azure Container Registry.

Example:

```text
Registry: ShreeyeshContainerRegistry
Repository: demowebapplication
Tag: v1

Full Image:
shreeyeshcontainerregistry.azurecr.io/demowebapplication:v1
```

Now we need to tell Azure App Service which Docker image it should run.

Execute:

```cmd
az webapp config container set -g Demo-Resource-Group -n Demo-Service-Plan --container-image-name shreeyeshcontainerregistry.azurecr.io/demowebapplication:v1 --container-registry-url https://shreeyeshcontainerregistry.azurecr.io
```

This configures the Docker image in our App Service.

### Verify Image Configuration

```cmd
az webapp config show -g Demo-Resource-Group -n Demo-Service-Plan --query linuxFxVersion -o tsv
```

Expected output:

```text
DOCKER|shreeyeshcontainerregistry.azurecr.io/demowebapplication:v1
```

**Important:** The image tag must match the actual tag available in ACR. In our case, the tag is `v1` (lowercase).

---

## 5. Configure Managed Identity for ACR Authentication

### Why do we need authentication?

Our Azure Container Registry is private.

Therefore, Azure App Service needs authentication before it can download Docker images from ACR.

One option is to provide the ACR username and password.

However, Azure provides a better approach called **Managed Identity**.

### What is Managed Identity?

Managed Identity is an identity automatically managed by Azure for an Azure resource.

It allows one Azure service to authenticate with another Azure service without storing credentials in application configuration.

In our scenario:

- App Service has a System Assigned Managed Identity.
- ACR needs to recognize and authorize that identity.
- We assign the `AcrPull` role to the App Service identity.

### Step 1: Enable System Assigned Managed Identity

Navigate to:

```text
Azure Portal
    → App Service
        → Identity
            → System assigned
                → Enable
                → Save
```

Azure generates a unique identity for the App Service.

### Step 2: Enable Managed Identity Authentication for ACR

Execute:

```cmd
az webapp config set -g Demo-Resource-Group -n Demo-Service-Plan --generic-configurations acrUseManagedIdentityCreds=true
```

This tells App Service to use Managed Identity credentials when pulling images from ACR.

### Step 3: Assign AcrPull Permission

Now App Service needs permission to pull images from ACR.

Navigate to:

```text
Azure Portal
    → Container Registry
        → Access control (IAM)
            → Add role assignment
```

Configure:

- Role: `AcrPull`
- Assign access to: Managed Identity
- Select your App Service.

Alternatively, execute:

```cmd
az role assignment create --assignee YOUR_MANAGED_IDENTITY_PRINCIPAL_ID --role AcrPull --scope YOUR_ACR_RESOURCE_ID
```

To retrieve the ACR resource ID:

```cmd
az acr show --name ShreeyeshContainerRegistry --query id --output tsv
```

To retrieve the App Service Managed Identity principal ID:

```cmd
az webapp identity show -g Demo-Resource-Group -n Demo-Service-Plan --query principalId -o tsv
```

### What is AcrPull?

`AcrPull` is an Azure built-in role that grants permission to pull (download) container images from Azure Container Registry.

It does not grant permission to push or delete images.

**Note:** This role assignment applies to registries using the standard registry RBAC permission model. Registries using repository-level ABAC permissions may require a different repository reader role.

---

## 6. Configure Container Port

Our Dockerfile exposes port `8080`.

```dockerfile
EXPOSE 8080
```

Azure App Service needs to know which port inside the container receives incoming HTTP traffic.

We configure the following App Setting:

```text
WEBSITES_PORT = 8080
```

Execute:

```cmd
az webapp config appsettings set -g Demo-Resource-Group -n Demo-Service-Plan --settings WEBSITES_PORT=8080
```

### Why is this required?

Our application listens inside the container on port `8080`.

App Service needs to forward incoming HTTP requests to that container port.

**Important:** `WEBSITES_PORT` tells App Service which container port to route traffic to. It does not change the port on which the ASP.NET Core application listens. The application itself must be configured to listen on port `8080`.

---

## 7. Restart App Service

After configuring the Docker image, authentication and application port, restart the App Service.

```cmd
az webapp restart -g Demo-Resource-Group -n Demo-Service-Plan
```

App Service will attempt to pull the configured image from ACR and start the container.

---

## 8. Complete Deployment Architecture

```text
Developer
    |
    v
.NET 9 Web API
    |
    v
Docker Build
    |
    v
Docker Image
demowebapplication:v1
    |
    v
Docker Push
    |
    v
Azure Container Registry (ACR)
    |
    |  Managed Identity Authentication
    |  AcrPull Permission
    v
Azure App Service
    |
    v
Container Starts
    |
    v
.NET Application Running
    |
    v
Public Application URL
```

### Complete Deployment Flow

1. Developer builds a Docker image locally.
2. Developer pushes the image to Azure Container Registry.
3. Azure App Service is configured with the ACR image name and tag.
4. App Service authenticates with ACR using Managed Identity.
5. ACR checks whether the identity has `AcrPull` permission.
6. ACR allows App Service to download the image.
7. App Service starts a container using the downloaded image.
8. The .NET application starts listening on port `8080`.
9. Users access the application through the App Service public URL.

---

## 9. Verify Deployment

### Check configured Docker image

```cmd
az webapp config show -g Demo-Resource-Group -n Demo-Service-Plan --query linuxFxVersion -o tsv
```

### Configure container logging

```cmd
az webapp log config -g Demo-Resource-Group -n Demo-Service-Plan --docker-container-logging filesystem
```

### View container logs

```cmd
az webapp log tail -g Demo-Resource-Group -n Demo-Service-Plan
```

### Open the application

```text
https://YOUR_APP_SERVICE_NAME.azurewebsites.net
```

**Important:** App Service showing `Running` does not necessarily mean that the .NET application has started successfully. Always verify container logs and test an actual API endpoint.

---

## 10. Complete Deployment Commands

Run these commands in Windows CMD after replacing the two placeholders with the actual values retrieved from Azure.

```cmd
:: 1. Configure ACR Managed Identity authentication
az webapp config set -g Demo-Resource-Group -n Demo-Service-Plan --generic-configurations acrUseManagedIdentityCreds=true

:: 2. Configure Docker image
az webapp config container set -g Demo-Resource-Group -n Demo-Service-Plan --container-image-name shreeyeshcontainerregistry.azurecr.io/demowebapplication:v1 --container-registry-url https://shreeyeshcontainerregistry.azurecr.io

:: 3. Assign AcrPull permission
az role assignment create --assignee YOUR_MANAGED_IDENTITY_PRINCIPAL_ID --role AcrPull --scope YOUR_ACR_RESOURCE_ID

:: 4. Configure application port
az webapp config appsettings set -g Demo-Resource-Group -n Demo-Service-Plan --settings WEBSITES_PORT=8080

:: 5. Restart App Service
az webapp restart -g Demo-Resource-Group -n Demo-Service-Plan
```

---

## 11. Important Concepts - Quick Revision

| Concept | Meaning |
|---|---|
| Azure App Service | Managed hosting platform for web applications and APIs |
| App Service Plan | Defines compute resources and pricing tier |
| Custom Container | Runs an application packaged inside a Docker image |
| Managed Identity | Allows Azure resources to authenticate without storing credentials |
| AcrPull | Permission to download images from ACR |
| WEBSITES_PORT | Configures the container's incoming application port |
| LinuxFxVersion | Contains the configured Linux runtime or custom container image |

## 12. Important Takeaway

- **Docker** builds and manages container images.
- **ACR** stores Docker images.
- **Managed Identity** provides secure authentication between Azure resources.
- **AcrPull** grants permission to download images from ACR.
- **Azure App Service** pulls the Docker image and runs the application.
- **WEBSITES_PORT** tells App Service which port to forward incoming requests to.

**Remember:** We are not uploading our source code directly to App Service. We upload a Docker image to ACR, and App Service pulls that image and runs it as a container.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/7d01acf2-adae-474c-aae6-42305ec2fafc" />
