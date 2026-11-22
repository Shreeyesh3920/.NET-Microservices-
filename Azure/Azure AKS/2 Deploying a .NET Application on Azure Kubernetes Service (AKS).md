# Deploying a .NET Application on Azure Kubernetes Service (AKS)

## 1. Overview

In this deployment, we hosted our .NET Weather Forecast application on Azure Kubernetes Service (AKS).

The overall workflow:

```text
.NET Application
       |
       v
   Dockerfile
       |
       v
  Docker Image
       |
       v
Push Image to Azure Container Registry (ACR)
       |
       v
 Azure Kubernetes Service (AKS)
       |
       v
 Kubernetes Deployment
       |
       v
      Pod
       |
       v
 Kubernetes Service (LoadBalancer)
       |
       v
 Public IP Address
       |
       v
 Access Application from Browser
```

## 2. Prerequisites

The following tools and resources are required:

- Azure CLI
- Docker Desktop
- kubectl
- Azure Container Registry (ACR)
- Azure Kubernetes Service (AKS)

Our Azure resources:

| Resource | Name |
|---|---|
| Resource Group | `Demo-Resource-Group` |
| Container Registry | `shreeyeshcontainerregistry` |
| ACR Login Server | `shreeyeshcontainerregistry.azurecr.io` |
| Image Repository | `demowebapplication` |
| AKS Cluster | `Demo-Kubernete-Cluster` |

## 3. Build the Docker Image

First, we containerize our .NET application using its Dockerfile.

Navigate to the project directory:

```bash
cd C:\Users\shree\source\repos\DemoWebApplication
```

Build the Docker image:

```bash
docker build -t demowebapplication:v1 .
```

Verify the image:

```bash
docker images
```

**What happens here?**

- Docker reads instructions from the Dockerfile.
- It builds the application and packages its dependencies.
- It creates a Docker image that can be used to run containers.

## 4. Push the Docker Image to Azure Container Registry (ACR)

AKS needs access to the Docker image. We store our image in Azure Container Registry.

### Step 1: Login to Azure

```bash
az login
```

### Step 2: Login to ACR

```bash
az acr login --name shreeyeshcontainerregistry
```

### Step 3: Tag the Docker image

An image must have the ACR login server as its registry prefix before pushing it.

```bash
docker tag demowebapplication:v1 shreeyeshcontainerregistry.azurecr.io/demowebapplication:v1
```

### Step 4: Push the image

```bash
docker push shreeyeshcontainerregistry.azurecr.io/demowebapplication:v1
```

### Step 5: Verify the image in ACR

```bash
az acr repository list --name shreeyeshcontainerregistry --output table
```

Check the image tags:

```bash
az acr repository show-tags --name shreeyeshcontainerregistry --repository demowebapplication --output table
```

**What happens here?**

Our Docker image is uploaded to ACR. Instead of relying on a locally available image, AKS can pull the image from the Azure Container Registry.

## 5. Create and Configure AKS Cluster

AKS is Azure's managed Kubernetes service.

It provides a Kubernetes cluster where we can deploy and manage our containerized applications.

We created the following cluster:

- Cluster name: `Demo-Kubernete-Cluster`
- Resource Group: `Demo-Resource-Group`
- Region: `indiasouthcentral`

### Step 1: Connect to the AKS cluster

```bash
az aks get-credentials --resource-group Demo-Resource-Group --name Demo-Kubernete-Cluster
```

This downloads and merges the cluster credentials into the local kubeconfig file.

It allows `kubectl` to communicate with our AKS cluster.

### Step 2: Verify the connection

```bash
kubectl get nodes
```

Example output:

```text
NAME                                  STATUS
aks-agentpool-35815686-vmss000000     Ready
aks-agentpool-35815686-vmss000001     Ready
```

The `Ready` status indicates that the worker nodes are ready to run workloads.

### Step 3: Configure ACR access

AKS must have permission to pull images from ACR.

For a compatible Azure RBAC configuration, we can attach ACR to AKS using:

```bash
az aks update --resource-group Demo-Resource-Group --name Demo-Kubernete-Cluster --attach-acr shreeyeshcontainerregistry
```

This configures the required `AcrPull` permission for the AKS kubelet identity.

If this method is not supported by the registry's permission configuration, configure the appropriate image-pull permissions separately.

## 6. Create Kubernetes Deployment.yaml

Now that our image is available in ACR and AKS is configured, we need to deploy our application.

A Kubernetes Deployment manages the lifecycle of Pods.

It is responsible for:

- Creating Pods.
- Maintaining the desired number of replicas.
- Recreating Pods if they fail.
- Managing application updates and rollouts.

Create a file named `Deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: weather-forecast-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: weather-forecast
  template:
    metadata:
      labels:
        app: weather-forecast
    spec:
      containers:
        - name: weather-forecast-container
          image: shreeyeshcontainerregistry.azurecr.io/demowebapplication:v1
          imagePullPolicy: Always
          ports:
            - containerPort: 8080
              protocol: TCP
```

### Understanding Deployment.yaml

| Property | Description |
|---|---|
| `apiVersion` | Kubernetes API version used for Deployment |
| `kind` | Specifies the resource type |
| `metadata.name` | Name of the Deployment |
| `replicas` | Number of Pod replicas to maintain |
| `selector.matchLabels` | Identifies Pods managed by this Deployment |
| `template` | Defines the Pod configuration |
| `template.metadata.labels` | Assigns labels to Pods |
| `containers.name` | Container name inside the Pod |
| `containers.image` | Docker image pulled from ACR |
| `containerPort` | Documents the application port inside the container |

**Important:** Our ASP.NET Core application logs showed:

```text
Now listening on: http://[::]:8080
```

Therefore, we use port `8080`. The `containerPort` field itself does not configure the application's listening port.

### Apply the Deployment

```bash
kubectl apply -f Deployment.yaml
```

### Verify the Deployment

```bash
kubectl get deployments
```

### Verify the Pods

```bash
kubectl get pods
```

Example output:

```text
NAME                                          READY   STATUS
weather-forecast-deployment-9b7d9cb49-4sgzw   1/1     Running
```

The `Running` status indicates that the Pod has started.

Check application logs:

```bash
kubectl logs deployment/weather-forecast-deployment
```

## 7. Create Kubernetes Service.yaml

After creating the Deployment, our application is running inside a Pod.

However, we still need a way to access the application through the network.

This is where a Kubernetes Service comes into the picture.

A Service provides a stable network endpoint and forwards traffic to matching Pods.

Create a file named `Service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: weather-forecast-service
spec:
  type: LoadBalancer
  selector:
    app: weather-forecast
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

### Understanding Service.yaml

| Property | Description |
|---|---|
| `apiVersion` | Kubernetes API version |
| `kind` | Specifies the resource type |
| `metadata.name` | Name of the Service |
| `type: LoadBalancer` | Requests an external load balancer |
| `selector` | Identifies the Pods receiving traffic |
| `protocol` | Network protocol used |
| `port` | Port exposed by the Service |
| `targetPort` | Actual application port inside the Pod |

### How does the Service identify the Pod?

Notice that we did not specify any Pod name in Service.yaml.

In Deployment.yaml, we assigned this label:

```yaml
template:
  metadata:
    labels:
      app: weather-forecast
```

In Service.yaml, we use the same label:

```yaml
selector:
  app: weather-forecast
```

Kubernetes uses this selector to automatically discover all matching Pods.

For example, if our Deployment has three replicas, the Service can route traffic to all three matching Pods.

This is called **label-based Pod selection**.

### Apply the Service

```bash
kubectl apply -f Service.yaml
```

### Verify the Service

```bash
kubectl get services
```

Example output:

```text
NAME                       TYPE           CLUSTER-IP      EXTERNAL-IP
kubernetes                 ClusterIP      10.0.0.1        <none>
weather-forecast-service   LoadBalancer   10.0.214.221    172.198.4.123
```

The `EXTERNAL-IP` is assigned by Azure's load balancer.

Once it is assigned, our application can be accessed externally.

## 8. Access the Application

Open the browser and navigate to:

```text
http://172.198.4.123/WeatherForecast
```

Replace the IP with the actual external IP returned by:

```bash
kubectl get services
```

Our traffic flow looks like this:

```text
       External User
             |
             v
   Azure Load Balancer
      Public IP : 80
             |
             v
 Kubernetes Service
       Port : 80
             |
             v
     Service Selector
   app=weather-forecast
             |
             v
     Weather Forecast Pod
             |
             v
    ASP.NET Core Application
         Port : 8080
```

The Service forwards incoming requests from port `80` to port `8080` inside the selected Pod.

## 9. Useful Kubernetes Commands

| Command | Purpose |
|---|---|
| `kubectl get nodes` | List cluster nodes |
| `kubectl get deployments` | List Deployments |
| `kubectl get pods` | List Pods |
| `kubectl get services` | List Services |
| `kubectl describe pod <pod-name>` | Inspect a Pod |
| `kubectl describe service <service-name>` | Inspect a Service |
| `kubectl logs deployment/<deployment-name>` | View application logs |
| `kubectl get endpoints <service-name>` | Check Service endpoints |
| `kubectl get endpointslices` | View EndpointSlices |
| `kubectl apply -f Deployment.yaml` | Create or update Deployment |
| `kubectl apply -f Service.yaml` | Create or update Service |
| `kubectl delete -f Deployment.yaml` | Delete Deployment |
| `kubectl delete -f Service.yaml` | Delete Service |

## 10. Deployment vs Pod vs Service

These are three important Kubernetes concepts.

### Deployment

A Deployment manages the desired state of our application.

For example, if we configure:

```yaml
replicas: 3
```

Kubernetes attempts to maintain three Pods.

### Pod

A Pod is the smallest deployable unit in Kubernetes.

It runs one or more containers. In our example, the Weather Forecast Pod runs our application container.

### Service

A Service provides a stable network endpoint and routes traffic to Pods selected through labels.

Pods can be recreated and receive new IP addresses. The Service provides a stable way to reach the application without depending on individual Pod IPs.

## 11. Final Architecture

```text
                Azure Cloud
                     |
          +----------+----------+
          |                     |
          v                     v
    Azure Container       Azure Kubernetes
     Registry (ACR)        Service (AKS)
          |                     |
          |                     v
          |               Kubernetes
          |                 Cluster
          |                     |
          |                     v
          |                 Deployment
          |                     |
          |                     v
          |                    Pod
          |                     |
          |                     v
          |              Docker Container
          |              Weather Forecast
          |                     |
          |                     v
          |                 Application
          |                  Port 8080
          |                     ^
          |                     |
          |               Kubernetes
          |                 Service
          |                     |
          |                     v
          +--------------> LoadBalancer
                                |
                                v
                          External IP
                                |
                                v
                             Browser
```

## 12. Key Takeaways

1. **Docker:** Packages our application into a container image.
2. **ACR:** Stores our Docker images in Azure.
3. **AKS:** Provides a managed Kubernetes cluster to run containerized applications.
4. **Deployment:** Manages Pods and maintains the desired number of replicas.
5. **Pod:** Runs our application container.
6. **Service:** Provides a stable network endpoint and routes traffic to matching Pods.
7. **LoadBalancer:** Exposes the application externally through an Azure load balancer and public IP.

**Remember the complete flow:**

`Code → Docker Image → ACR → AKS → Deployment → Pod → Service → External IP → Browser`
