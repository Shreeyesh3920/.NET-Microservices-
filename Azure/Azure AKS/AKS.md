BHAI! 😂 Ab ekdum zero se start karte hain. Is baar sirf commands ya YAML ratwayenge nahi. Main tujhe Azure + Docker + Kubernetes + AKS + Deployment YAML + Service YAML + LoadBalancer + ClusterIP + ACR + Pods + Services + Networking + actual deployment flow sab kuch aise samjhaunga ki 4–5 mahine baad bhi notes kholega toh poora architecture samajh aa jayega.

Hum apne actual eCommerce Microservices project ko example banayenge. Jahan humne practically kuch kiya hai, wahan bataunga humne kya kiya. Jahan abhi kaam pending hai, wahan clearly bataunga abhi kya nahi kiya. Dono ko mix nahi karenge.

### PART 1 — Sabse pehle samajh: Hum bana kya rahe hain?

Humara project ek eCommerce application hai jisme multiple independent microservices hain.

Client / User

Browser, Postman, frontend application

API Gateway

Public entry point — Ocelot

Public IP: 4.155.89.160

User Service

Authentication

Product Service

Products

Order Service

Orders

Supporting infrastructure

PostgreSQL

MySQL

MongoDB

Redis

RabbitMQ

Conceptual architecture of our eCommerce application. PostgreSQL deployment and some service integrations are still pending verification.

Humare paas alag-alag services hain:

| Component       | Kaam                                                                         |
| --------------- | ---------------------------------------------------------------------------- |
| API Gateway     | External requests receive karta hai aur correct service ko forward karta hai |
| User Service    | Register, Login, User details                                                |
| Product Service | Products manage karta hai                                                    |
| Order Service   | Orders manage karta hai                                                      |
| MySQL           | Product data                                                                 |
| PostgreSQL      | User data                                                                    |
| MongoDB         | Order data                                                                   |
| Redis           | Caching                                                                      |
| RabbitMQ        | Asynchronous messaging                                                       |

Microservices ka main idea: har service ka apna responsibility hota hai. Product Service ko User Service ke internal code ki zaroorat nahi hoti. Dono APIs ke through communicate karte hain.

Ab sawaal: in sab services ko internet par available kaise karenge, aur agar ek service ke multiple instances chalane hain toh kaise manage karenge?

Yahin se Docker aur Kubernetes aate hain.

### PART 2 — Docker se Azure tak: Poora picture

### 1\. Docker kya karta hai?

Maan le humne Product Service C# .NET mein banayi.

Normally application run karne ke liye machine par chahiye:

* .NET runtime

* Required libraries

* Application DLLs

* Configuration

* Environment variables

Problem: mere laptop par chal raha hai, lekin doosre server par runtime ya dependency missing hai toh application fail ho sakti hai.

Docker is problem ko solve karta hai.

### Dockerfile → Image → Container

Dockerfile

Application ko package kaise karna hai, uski instructions

Docker Image

Application + runtime + dependencies ka packaged template

Container

Image ka running instance

Example:

```cmd
docker build -t product-service:v1 .
```

Iska matlab:

* `docker build`: Docker image build kar.

* `-t`: image ko tag/name de.

* `product-service:v1`: image ka naam aur version.

* `.`: current directory ko build context bana.

Container run karne ke liye:

```cmd
docker run -d -p 8080:8080 product-service:v1
```

Yahan `-p 8080:8080` ka matlab host machine ke port 8080 ko container ke port 8080 se map karna.

Important: Docker container chalata hai. Lekin agar 20 containers manage karne hain, crash hone par restart karna hai, scaling karni hai, networking manage karni hai, health check karna hai — toh sirf Docker enough nahi hota.

Isliye Kubernetes.

### 2\. Kubernetes kya karta hai?

Kubernetes ek container orchestration platform hai.

Simple language mein:

Docker = container banane aur run karne ka tool.

Kubernetes = bahut saare containers ko manage karne ka system.

Maan le hum chahte hain:

* Product Service ke 2 instances.

* User Service ke 2 instances.

* Order Service ke 2 instances.

* API Gateway ke 2 instances.

* Agar ek Pod crash ho, toh replacement automatically create ho.

* Application ko public IP se access kar sakein.

* Services ek doosre ko stable names se call kar sakein.

Kubernetes ye sab manage karta hai.

Kubernetes ko humne khud install nahi kiya. Humne Azure ka managed Kubernetes service use kiya: AKS.

### 3\. AKS kya hai?

![Azure AKS - Opsera](https://images.openai.com/static-rsc-4/zJ4J6LcpsAfZ7SHA2V_mJCIqD2au8vRMoaViZgtCiL5KtXLquusQefLeAlIADKA280lyoAfI0CmQQ38j6dW1oOPzaCCHbg1yMWy7Yo77Skmsd_v2cVbonccuS8jvZ7T5QR7KczjeatHEsEFc5NNbXVSJMlWHsp22dtudXY1IU6b3MyWlpmegYhyXfnVIttW6?purpose=fullsize)

Azure Kubernetes Service (AKS)

Microsoft Azure ka managed Kubernetes service.

Kubernetes open-source technology hai. AKS Azure par Kubernetes cluster create aur manage karne ka service hai.

AKS mein broadly do parts samajh:

Control Plane: cluster ka brain. Ye decide karta hai kya run hona chahiye, Pods kahan schedule hone chahiye, aur desired state maintain kaise karni hai.

Worker Nodes: Azure virtual machines jahan actual application Pods run hote hain.

Azure AKS ke control plane ko manage karta hai. Hum application deployments, services, configurations aur cluster ke workloads manage karte hain.

### PART 3 — Azure par humne kya-kya create kiya?

Ab hum apne actual Azure setup ko samajhte hain.

![Data Platform: Building An Azure DataHub | PTR](https://images.openai.com/static-rsc-4/f0OLWOHR-P3PaEpMJ_EuKmbB8SBAPd3Dg6HJRh3neV5EpZ0Rc0RYTyUvmCudUUlYYRkOVVx8qwTGHRXmtQwaNRpeLtPDhaDE5-Yljn1TrWgRO7tPCYzyK2oM7hqNrrQ5TcPkz4BJUD8RmidpWS7ZyPalibDwcDTZVTEojPBz1nMfhicByiSkLDRKP06v5i0p?purpose=fullsize)

Microsoft Azure

Cloud platform

Resource Group

Demo-Resource-Group

Azure resources ko logically organize karne ka container.

AKS Cluster

eCommerceAppCluster

Container Registry

shreeyeshcontainerregistry

### 1\. Resource Group

Humara Resource Group:

```text
Demo-Resource-Group
```

Resource Group ka matlab Azure resources ko ek logical group mein rakhna.

Example: ek project ke liye AKS, Container Registry, Storage Account, Public IP, etc. ko ek Resource Group mein organize kar sakte hain.

Important:

* Resource Group Kubernetes namespace nahi hai.

* Resource Group application container nahi hai.

* Ye Azure resources ka management boundary hai.

### 2\. Azure Container Registry (ACR)

![Azure Container Registry Icon & Logo | Dashboard Icons & Logos](https://images.openai.com/static-rsc-4/3CTZQRiwfB0_x3-7I0eDSCr_DhUIzq9m8Q7n3kiVOeOOUPODpEmY9e5TRc_rHOQ0yC88drXCa1-12cHMLMR-SLBQYJvVQIu78O3MnvJU8P9uNbMbLVtoRRDEypx5JQawlQFePPamqDGz1DNKSRyVYdhr1NVrDMudUVVZrtMf7Sthwq-fJSkGP24S8HYnrLIS?purpose=fullsize)

ACR — Azure Container Registry

Docker images store karne ki private registry.

Humne ACR create kiya:

```text
shreeyeshcontainerregistry
```

Registry ka login server:

```text
shreeyeshcontainerregistry.azurecr.io
```

Ab samajh: hum apne laptop par Docker images build karte hain. Lekin AKS cluster Azure mein hai. AKS ko image kahan se milegi?

ACR se.

Humne ye images push ki:

```text
shreeyeshcontainerregistry.azurecr.io/api-gateway:v1
shreeyeshcontainerregistry.azurecr.io/api-gateway:v2

shreeyeshcontainerregistry.azurecr.io/user-service:v1

shreeyeshcontainerregistry.azurecr.io/product-service:v1

shreeyeshcontainerregistry.azurecr.io/order-service:v1
```

Example command:

```cmd
az acr login --name shreeyeshcontainerregistry
```

Phir:

```cmd
docker push shreeyeshcontainerregistry.azurecr.io/product-service:v1
```

Iska matlab: local Docker image ko Azure Container Registry mein upload karna.

ACR aur AKS alag cheezein hain.

* ACR = images store karta hai.

* AKS = images ko pull karke Pods mein run karwata hai.

Humne AKS ko ACR se connect bhi kiya:

```cmd
az aks update --name eCommerceAppCluster --resource-group Demo-Resource-Group --attach-acr shreeyeshcontainerregistry
```

Ye AKS identity ko ACR se images pull karne ki permission deta hai. Isse image pull ke liye manually har Deployment mein username/password daalne ki zaroorat nahi padti.

### 3\. AKS Cluster

Humara cluster:

```text
Cluster Name: eCommerceAppCluster
Resource Group: Demo-Resource-Group
Region: westus2
Kubernetes Version: 1.36.5
```

Cluster create hone ke baad humne local machine ko us cluster se connect kiya:

```cmd
az aks get-credentials --resource-group Demo-Resource-Group --name eCommerceAppCluster
```

Is command ka kya kaam?

Azure CLI AKS cluster ki connection credentials aur configuration local kubeconfig mein add karta hai. Uske baad `kubectl` se cluster manage kar sakte hain.

Verify:

```cmd
kubectl config current-context
```

Nodes check:

```cmd
kubectl get nodes
```

Pods check:

```cmd
kubectl get pods
```

Ek important distinction: `az` Azure resources manage karta hai; `kubectl` Kubernetes cluster ke resources manage karta hai.

| Command              | Kaam                                       |
| -------------------- | ------------------------------------------ |
| az group ...         | Azure Resource Group                       |
| az acr ...           | Azure Container Registry                   |
| az aks ...           | AKS cluster                                |
| kubectl get pods     | Kubernetes Pods                            |
| kubectl apply -f ... | YAML se Kubernetes resources create/update |
| kubectl get services | Kubernetes Services                        |

### PART 4 — Kubernetes cluster ke andar actually kya hota hai?

Ab ye interview ka bahut important topic hai.

AKS cluster ko do major parts mein divide kar:

![Medium](https://images.openai.com/static-rsc-4/C3sdNZB86KrYDlGbuCymh7bQNnxVRRHacPyATgIRMbW7w7pECs0ZL8KAyJO_bY3UWZxjhcLLOemLNpFPh0tG1p2H26HrNoOQe7WDkHsmx5AiEZ95QwNAkTbjmMr5-wOijG86IG9gGvq_d7c4A8AfqbjYx-0TH0ImxTPVbfIsx1oMUlRs2j63ZvrTCUjbL4Fn?purpose=fullsize)

### 1\. Control Plane — Cluster ka brain

Control Plane decide karta hai cluster mein kya hona chahiye.

Iske important components:

### API Server

Ye Kubernetes ka entry point hai.

Jab hum command chalate hain:

```cmd
kubectl apply -f deployment.yaml
```

Toh `kubectl` directly Pod create nahi karta. Ye Kubernetes API Server ko request bhejta hai.

API Server:

* Request receive karta hai.

* Validate karta hai.

* Authentication aur authorization check karta hai.

* Desired configuration Kubernetes state mein save karwata hai.

Simple example: tu bolta hai, "Mujhe Product Service ke 2 replicas chahiye."

Ye request API Server ke through cluster tak jaati hai.

### etcd

etcd ek distributed key-value database hai jo Kubernetes cluster ki state store karta hai.

Ismein cluster ke resources ki configuration/state store hoti hai.

Example:

* Deployment mein replicas kitne hone chahiye?

* Service ka configuration kya hai?

* Kaunse resources exist karte hain?

Interview mein bol sakta hai:

"etcd is the backing datastore of Kubernetes. It stores the cluster's configuration and state."

### Scheduler

Scheduler decide karta hai ki newly created Pod kis Worker Node par run hoga.

Maan le cluster mein 2 worker nodes hain:

* Node 1

* Node 2

Aur ek naya Product Service Pod create karna hai.

Scheduler available resources aur scheduling rules dekhkar suitable Node select karta hai.

### Controller Manager

Ye controllers run karta hai jo desired state aur actual state compare karte hain.

Example:

Deployment mein likha:

```yaml
replicas: 2
```

Lekin actual mein sirf 1 Pod running hai.

Controller notice karega ki desired state 2 hai, actual state 1. Phir missing Pod create karne ka process trigger karega.

Yahi Kubernetes ka reconciliation concept hai.

Desired state = jo humne YAML mein declare kiya.

Actual state = cluster mein currently jo ho raha hai.

Kubernetes continuously dono ko match karne ki koshish karta hai.

### 2\. Worker Node — Jahan application run hoti hai

Worker Node ek Azure VM hoti hai jo cluster ke workloads run karti hai.

Important components:

* Kubelet: Node ka agent. API Server se instructions lekar Pods aur containers ki health/status manage karta hai.

* Container Runtime: Container run karta hai, jaise containerd.

* Kube-proxy / Service networking: Service traffic ko appropriate Pods tak pahunchane mein help karta hai. AKS networking configuration ke hisaab se data plane implementation differ kar sakta hai.

Ab ek simple example:

Humne Product Service ke 2 replicas maange.

Kubernetes ka flow:

* Hum Deployment YAML apply karte hain.

* API Server configuration accept karta hai.

* Deployment Controller desired replicas maintain karne ke liye ReplicaSet ensure karta hai.

* ReplicaSet 2 Pods ensure karta hai.

* Scheduler un Pods ke liye Nodes choose karta hai.

* Kubelet container runtime ke through containers run karwata hai.

### PART 5 — Pod, Deployment, ReplicaSet: ye teen kya hain?

Ye samajh gaya toh Kubernetes ka aadha confusion khatam.

### 1\. Pod kya hai?

Pod Kubernetes ki smallest deployable unit hai.

Humne Product Service ke 2 Pods chalaye.

Example:

```text
product-service-deployment
        |
        v
    ReplicaSet
       / \
      /   \
   Pod 1  Pod 2
```

Har Pod mein humare case mein ek application container hai.

Important:

* Pod aur container same cheez nahi hain.

* Ek Pod mein one or more containers ho sakte hain.

* Pod ka apna IP hota hai.

* Pod replace ho sakta hai, aur replacement Pod ko naya IP mil sakta hai.

Isliye hum directly Pod IP ko permanent endpoint ki tarah use nahi karte. Hum Service use karte hain.

### 2\. Deployment kya hai?

Deployment ek Kubernetes resource hai jo stateless application Pods ko manage karne mein help karta hai.

Example:

```yaml
replicas: 2
```

Iska matlab Kubernetes ko 2 matching Pods maintain karne hain.

Agar:

* Pod crash ho gaya.

* Pod delete ho gaya.

* Node unavailable ho gaya.

Toh Deployment ke through managed ReplicaSet desired number of Pods maintain karne ki koshish karega.

Deployment humein ye bhi provide karta hai:

* Rolling updates

* Rollback

* Replica management

* Declarative updates

### 3\. ReplicaSet kya hai?

ReplicaSet ka main kaam desired number of Pods maintain karna hai.

Normally hum directly ReplicaSet YAML nahi likhte. Hum Deployment banate hain aur Deployment automatically ReplicaSet manage karta hai.

Example:

```text
Deployment
    |
    v
ReplicaSet
    |
    +---- Pod 1
    |
    +---- Pod 2
```

Interview answer:

"Deployment manages ReplicaSets, and ReplicaSets ensure that the desired number of Pods are running."

### PART 6 — Ab asli important cheez: deployment.yaml

Bhai, ab dhyaan se. `deployment.yaml` Kubernetes ko batata hai:

* Kaunsi application run karni hai?

* Kaunsi Docker image use karni hai?

* Kitne replicas chahiye?

* Container ka port kya hai?

* Environment variables kya hain?

* Secrets kahan se lene hain?

Humare Product Service Deployment ka representative example:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: product-service-deployment

spec:
  replicas: 2

  selector:
    matchLabels:
      app: product-service

  template:
    metadata:
      labels:
        app: product-service

    spec:
      containers:
        - name: product-service

          image: shreeyeshcontainerregistry.azurecr.io/product-service:v1

          ports:
            - containerPort: 8080

          env:
            - name: ASPNETCORE_ENVIRONMENT
              value: "Development"

            - name: ASPNETCORE_URLS
              value: "http://+:8080"

            - name: MYSQL_HOST
              value: "mysql"

            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-secret
                  key: MYSQL_ROOT_PASSWORD

            - name: REDIS_CONNECTION_STRING
              value: "redis:6379"

            - name: RabbitMQConfiguration__HostName
              value: "rabbitmq"
```

Ye learning example hai; ismein humare actual deployment ke kuch environment variables abbreviated hain. Actual file mein RabbitMQ port, username aur password bhi configure kiye gaye the.

Ab ek-ek line samajh.

### `apiVersion`

```yaml
apiVersion: apps/v1
```

Kubernetes API ka version batata hai jiske through resource define ho raha hai.

Deployment ke liye commonly `apps/v1` use hota hai.

### `kind`

```yaml
kind: Deployment
```

Ye sabse important fields mein se ek hai.

Kubernetes ko batata hai ki hum kaunsa resource create karna chahte hain.

Examples:

```yaml
kind: Deployment
```

Application Pods manage karne ke liye.

```yaml
kind: Service
```

Pods ko stable network endpoint dene ke liye.

```yaml
kind: Secret
```

Sensitive configuration store karne ke liye.

```yaml
kind: ConfigMap
```

Non-sensitive configuration store karne ke liye.

`kind` ka matlab resource type hai. Ye LoadBalancer nahi hai. LoadBalancer Service ke `spec.type` mein aata hai. Ye distinction yaad rakhna!

### `metadata`

```yaml
metadata:
  name: product-service-deployment
```

Resource ka naam.

Hum ise command se retrieve kar sakte hain:

```cmd
kubectl get deployment
```

### `spec`

```yaml
spec:
  replicas: 2
```

`spec` resource ka desired configuration batata hai.

`replicas: 2` ka matlab do matching Pods maintain karne hain.

Maan le ab tu replicas 2 se 3 kar de:

```yaml
replicas: 3
```

Aur apply kare:

```cmd
kubectl apply -f deployment.yaml
```

Toh Kubernetes ek additional Pod create karne ki koshish karega.

### `selector`

```yaml
selector:
  matchLabels:
    app: product-service
```

Deployment ko batata hai ki woh kaunse labels wale Pods manage karega.

### `template`

```yaml
template:
  metadata:
    labels:
      app: product-service
```

Ye Pod template hai.

Deployment jab bhi Pod create karega, us Pod par ye label lagayega.

Selector aur Pod labels match hone chahiye.

Dekho:

Deployment selector `app: product-service`

Pod 1

app: product-service

Match

Pod 2

app: product-service

Match

Agar selector aur labels match nahi hue toh Deployment expected Pods ko manage nahi kar payega.

### `containers`

```yaml
containers:
  - name: product-service
```

Pod ke andar kaunsa container run karna hai.

### `image`

```yaml
image: shreeyeshcontainerregistry.azurecr.io/product-service:v1
```

Ye humare ACR mein stored Docker image ka full address hai.

Kubernetes worker node ka container runtime is image ko registry se pull karta hai, aur phir container start hota hai.

Isliye humne ACR ko AKS se attach kiya tha.

### `containerPort`

```yaml
ports:
  - containerPort: 8080
```

Ye container ke andar application ka expected/listening port document karta hai.

Important: `containerPort` khud port publish nahi karta aur application ko automatically us port par listen bhi nahi karwata.

Humare .NET application ke liye:

```yaml
- name: ASPNETCORE_URLS
  value: "http://+:8080"
```

Is environment variable se ASP.NET Core ko port 8080 par listen karne ke liye configure kiya.

### `env`

```yaml
env:
  - name: REDIS_CONNECTION_STRING
    value: "redis:6379"
```

Application ke environment variables configure karne ke liye.

Humare Product Service ko pata hona chahiye Redis, MySQL aur RabbitMQ kahan available hain.

Yahan ek important Kubernetes concept:

```text
redis:6379
```

Humne Redis ke Pod ka IP nahi diya.

Humne Redis Kubernetes Service ka naam diya.

Kubernetes internal DNS ki help se `redis` ko resolve karta hai.

Isliye Pod replace ho jaaye toh application ko har baar naya Pod IP configure nahi karna padta.

### `secretKeyRef`

```yaml
valueFrom:
  secretKeyRef:
    name: mysql-secret
    key: MYSQL_ROOT_PASSWORD
```

Iska matlab:

* Kubernetes Secret ka naam `mysql-secret` hai.

* Us Secret ke andar key `MYSQL_ROOT_PASSWORD` hai.

* Uski value environment variable mein inject karo.

Humne Secret create kiya tha:

```cmd
kubectl create secret generic mysql-secret --from-literal=MYSQL_ROOT_PASSWORD=dev_mysql_password
```

RabbitMQ ke liye:

```cmd
kubectl create secret generic rabbitmq-secret --from-literal=RABBITMQ_PASSWORD=dev_rabbitmq_password
```

Security note: Kubernetes Secret automatically encrypted-at-rest guarantee nahi karta. Default representation Base64 hoti hai, jo encryption nahi hai. Production mein RBAC, encryption at rest aur Azure Key Vault integration jaise measures important hain.

### PART 7 — Ab services.yaml: Deployment hone ke baad Service kyun chahiye?

Ye concept bahut important hai.

Maan le Product Service ke do Pods hain:

```text
Pod 1 IP = 10.244.1.20
Pod 2 IP = 10.244.1.21
```

Ab Order Service ko Product Service call karni hai.

Agar Order Service directly Pod 1 ke IP ko call kare, toh problem:

* Pod 1 delete ho sakta hai.

* Replacement Pod ka IP different ho sakta hai.

* Pod 1 temporarily unavailable ho sakta hai.

* Doosra Pod available hone ke bawajood request us tak nahi jaayegi.

Toh humein ek stable endpoint chahiye.

Yahi Kubernetes Service provide karta hai.

### 1\. ClusterIP Service

Humare Product Service ka representative Service YAML:

```yaml
apiVersion: v1
kind: Service

metadata:
  name: product-service

spec:
  type: ClusterIP

  selector:
    app: product-service

  ports:
    - port: 8080
      targetPort: 8080
      protocol: TCP
```

Chalo samajhte hain.

### `apiVersion: v1`

Service core Kubernetes API group ka resource hai.

### `kind: Service`

Kubernetes ko bata rahe hain ki Service resource create karna hai.

### `metadata.name`

```yaml
name: product-service
```

Service ka naam.

Humare doosre microservices is naam ko internal hostname ki tarah use kar sakte hain.

Example:

```text
http://product-service:8080
```

### `type: ClusterIP`

```yaml
type: ClusterIP
```

Ye Service type batata hai.

ClusterIP ka matlab Service ko cluster ke andar accessible virtual IP dena.

Humne Product Service, User Service, Order Service, MySQL, MongoDB, Redis aur RabbitMQ ke liye internal ClusterIP Services use ki hain.

Ye public internet ke liye directly expose nahi hoti.

### `selector`

```yaml
selector:
  app: product-service
```

Service ko batata hai ki kaunse Pods tak traffic forward karna hai.

Ye label Product Service Deployment ke Pod template se match karta hai.

### `port`

```yaml
port: 8080
```

Service kis port par traffic accept karegi.

### `targetPort`

```yaml
targetPort: 8080
```

Service traffic ko selected Pod ke kis port par bhejegi.

Is example mein:

```text
Service port = 8080
Pod port     = 8080
```

Dono same hain, lekin zaroori nahi ki hamesha same hi hon.

Example:

```yaml
ports:
  - port: 80
    targetPort: 8080
```

Iska matlab:

* Service ko port 80 par call karo.

* Service traffic Pod ke port 8080 par forward karegi.

### 2\. Deployment aur Service ka connection

Product Service Deployment

replicas: 2

Pod 1

app: product-service

Port 8080

Pod 2

app: product-service

Port 8080

Service: product-service

ClusterIP — stable internal endpoint

Port 8080

Service selector matching Pods ko discover karta hai aur unke healthy/eligible endpoints tak traffic route karta hai.

Ek subtle but important baat: Service ke selector se matching Pods ke endpoints discover hote hain. Service networking implementation traffic ko un endpoints tak route karti hai. Ye zaroori nahi ki har request strict round-robin mein hi distribute ho.

### 3\. Humne Services kaise verify ki?

```cmd
kubectl get services
```

Detailed information:

```cmd
kubectl describe service product-service
```

Endpoints verify:

```cmd
kubectl get endpoints product-service
```

Newer Kubernetes mein EndpointSlice bhi inspect kar sakte hain:

```cmd
kubectl get endpointslices
```

Humne Product Service ke liye actual Pod endpoints dekhe the.

### PART 8 — Kubernetes Service ke 3 important types

Ab aata hai tera main sawaal: `kind`, `type`, `LoadBalancer` kya hai?

Sabse pehle difference:

```yaml
kind: Service
spec:
  type: LoadBalancer
```

* `kind: Service` = hum Service resource bana rahe hain.

* `type: LoadBalancer` = Service ka networking/exposure type LoadBalancer hai.

Kubernetes mein commonly 3 Service types samajh:

### 1\. ClusterIP

Internal access

```yaml
apiVersion: v1
kind: Service
metadata:
  name: product-service
spec:
  type: ClusterIP
  selector:
    app: product-service
  ports:
    - port: 8080
      targetPort: 8080
```

Use case:

Order Service ko Product Service call karna hai.

```text
http://product-service:8080
```

Ye internal communication ke liye hai.

Humne ye type use kiya:

* product-service

* user-service

* order-service

* mysql

* mongodb

* redis

* rabbitmq

### 2\. NodePort

Node-level exposure

NodePort Service cluster ke har eligible Node ke IP par ek port expose karti hai, usually default range `30000–32767` hoti hai.

Example:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: product-service
spec:
  type: NodePort
  selector:
    app: product-service
  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 30080
```

Maan le Node IP hai:

```text
20.x.x.x
```

Toh NodePort ke through request ka conceptual endpoint hoga:

```text
http://20.x.x.x:30080
```

NodePort ka matlab har Node par application ka separate Pod run hona nahi hai. Service networking request ko available matching Pod tak forward kar sakti hai.

### 3\. LoadBalancer

Public exposure — humne yahi use kiya

Ye humare API Gateway ke liye use hua.

Actual representative YAML:

```yaml
apiVersion: v1
kind: Service

metadata:
  name: api-gateway

spec:
  type: LoadBalancer

  selector:
    app: api-gateway

  ports:
    - port: 80
      targetPort: 8080
      protocol: TCP
```

Ab ek-ek field:

```yaml
type: LoadBalancer
```

Kubernetes ko request kar rahe hain ki is Service ko external load balancer ke through expose karo.

Azure AKS mein Azure cloud integration is request ko process karti hai aur Azure Load Balancer/Public IP infrastructure provision ya configure kar sakti hai.

Humare case mein public IP mila:

OUR API GATEWAY PUBLIC IP

### 4.155.89.160

Public URL: [http://4.155.89.160](http://4.155.89.160?utm_source=chatgpt.com)

 Copy API URL

`port: 80` ka matlab external Service port 80 hai.

`targetPort: 8080` ka matlab Service traffic Gateway Pod ke container port 8080 par bhejegi.

Humare Gateway container mein ASP.NET Core is tarah listen karta hai:

```yaml
- name: ASPNETCORE_URLS
  value: "http://+:8080"
```

Toh request ka flow:

```text
Internet
   |
   v
Azure Public IP
4.155.89.160
   |
   v
Azure Load Balancer
   |
   v
Kubernetes Service
api-gateway:80
   |
   v
API Gateway Pod:8080
```

Important: Azure Load Balancer aur Kubernetes Service same cheez nahi hain.

* Kubernetes Service `type: LoadBalancer` expose karne ki request/configuration hai.

* Azure Load Balancer Azure ka infrastructure component hai jo external traffic ko cluster tak pahunchata hai.

* Kubernetes Service phir matching Gateway Pods tak traffic route karne mein help karti hai.

### LoadBalancer vs ClusterIP vs NodePort

| Type         | Access             | Humne kahan use kiya                         |
| ------------ | ------------------ | -------------------------------------------- |
| ClusterIP    | Cluster ke andar   | Internal microservices, databases            |
| NodePort     | Node IP + NodePort | Humne primary exposure ke liye use nahi kiya |
| LoadBalancer | External IP        | API Gateway                                  |

### Toh humne sirf API Gateway ko LoadBalancer kyun banaya?

Kyunki ideally user ko har microservice directly internet par access nahi karna chahiye.

User ko ek public entry point chahiye:

```text
Internet
   |
   v
API Gateway
   |
   +---- User Service
   |
   +---- Product Service
   |
   +---- Order Service
```

Isse:

* External traffic ek central point par aata hai.

* Internal services private rehti hain.

* Routing, authentication, rate limiting aur policies centralize karna possible hota hai.

### PART 9 — Humare actual deployment ka request flow

Ab ye samajh ki humne `/products` endpoint kaise working banaya.

Postman / Browser / curl

GET [http://4.155.89.160/products](http://4.155.89.160/products?utm_source=chatgpt.com)

Azure Public IP + Load Balancer

4.155.89.160 : 80

Kubernetes Service: api-gateway

LoadBalancer Service → port 80 → target 8080

API Gateway Pod

Ocelot routing

Kubernetes Service: product-service

ClusterIP — product-service:8080

Product Pod 1

Port 8080

Product Pod 2

Port 8080

Redis + MySQL

Product Service ke backend dependencies

Actual Ocelot configuration mein humne Product route correct kiya tha:

```json
{
  "UpstreamHttpMethod": ["GET"],
  "UpstreamPathTemplate": "/products",
  "DownstreamPathTemplate": "/api/products",
  "DownstreamScheme": "http",
  "DownstreamHostAndPorts": [
    {
      "Host": "product-service",
      "Port": 8080
    }
  ]
}
```

Iska matlab:

Client call karta hai:

```http
GET /products
```

Ocelot internally forward karta hai:

```http
GET http://product-service:8080/api/products
```

Ab important detail:

`product-service` koi Pod IP nahi hai. Ye Kubernetes Service ka DNS name hai.

Pehle humare Ocelot configuration mein `localhost:7121` tha. Ye Docker Compose/local environment mein meaningful tha, lekin Kubernetes mein Gateway container ke andar `localhost` ka matlab Gateway ka apna container/Pod hota hai. Product Service ka Pod nahi.

Isliye connection refused hua.

Humne downstream hostname ko:

```text
product-service
```

se replace kiya, aur correct Gateway image v2 build karke ACR mein push ki.

Yahi actual deployment ka important debugging lesson hai.

### PART 10 — Humne actually Azure par deployment kaise kiya? Step by step

Ab tak concepts samajh gaye. Ab poora practical process dekh.

### Step 1: Application ko Docker image mein convert kiya

Har .NET microservice ke liye Dockerfile use kiya.

Example Product Service:

```cmd
docker build -t product-service:v1 .
```

Docker ne:

* Dockerfile read ki.

* .NET dependencies restore ki.

* Application publish/build ki.

* Image create ki.

### Step 2: Image ko ACR mein push kiya

Pehle login:

```cmd
az acr login --name shreeyeshcontainerregistry
```

Tag:

```cmd
docker tag product-service:v1 shreeyeshcontainerregistry.azurecr.io/product-service:v1
```

Push:

```cmd
docker push shreeyeshcontainerregistry.azurecr.io/product-service:v1
```

Image verify:

```cmd
az acr repository show-tags --name shreeyeshcontainerregistry --repository product-service --output table
```

Ab image Azure mein available hai.

### Step 3: AKS ko ACR permission di

```cmd
az aks update --name eCommerceAppCluster --resource-group Demo-Resource-Group --attach-acr shreeyeshcontainerregistry
```

Iske bina AKS worker nodes ko private ACR se image pull karne mein authorization issue aa sakta tha.

### Step 4: AKS cluster credentials local machine par laaye

```cmd
az aks get-credentials --resource-group Demo-Resource-Group --name eCommerceAppCluster
```

Verify:

```cmd
kubectl config current-context
kubectl get nodes
```

Ab humara local `kubectl` AKS cluster ko commands bhej sakta hai.

### Step 5: Secrets create kiye

MySQL password:

```cmd
kubectl create secret generic mysql-secret --from-literal=MYSQL_ROOT_PASSWORD=dev_mysql_password
```

RabbitMQ password:

```cmd
kubectl create secret generic rabbitmq-secret --from-literal=RABBITMQ_PASSWORD=dev_rabbitmq_password
```

Verify:

```cmd
kubectl get secrets
```

### Step 6: Database aur infrastructure deploy kiya

Humne infrastructure ke liye bhi Deployments aur Services create kiye:

* MySQL

* MongoDB

* Redis

* RabbitMQ

Example:

```cmd
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

Yahan humne database containers bhi Kubernetes Pods mein run kiye.

Important: database ko Pod mein run karna aur production-ready database architecture banana alag cheezein hain. Humare current setup mein persistent volumes configure nahi kiye gaye hain.

### Step 7: Microservices deploy ki

Humne in applications ke Deployment aur Service resources apply kiye:

* API Gateway

* User Service

* Product Service

* Order Service

Example:

```cmd
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### Step 8: API Gateway ko public expose kiya

Gateway ke Service YAML mein:

```yaml
type: LoadBalancer
```

Iske through Azure ne external LoadBalancer/Public IP provide kiya:

```text
4.155.89.160
```

### Step 9: Ocelot routing issue fix kiya

Pehle Ocelot Product Service ko:

```text
localhost:7121
```

par access kar raha tha.

Kubernetes mein ye wrong tha.

Correct downstream:

```text
product-service:8080
```

Humne Gateway image v2 rebuild/push ki aur Deployment update kiya.

Correct Docker build context bhi important tha. Humare Dockerfile ke paths ko dekhkar parent directory se build karna pada:

```cmd
cd /d "C:\Users\shree\OneDrive\Desktop\Microservices\eCommerceSolution.APIGateway"
docker build -t shreeyeshcontainerregistry.azurecr.io/api-gateway:v2 -f APIGateway\Dockerfile .
```

Push:

```cmd
docker push shreeyeshcontainerregistry.azurecr.io/api-gateway:v2
```

Deployment update ke baad:

```cmd
kubectl apply -f deployment.yaml
kubectl rollout status deployment/api-gateway-deployment
```

Rollout successful hua.

### Step 10: End-to-end Product endpoint test kiya

```cmd
curl -i http://4.155.89.160/products
```

Pehle:

* Ocelot downstream connection issue.

* Redis connection issue.

* MySQL table missing issue.

Humne Redis deploy kiya aur MySQL mein required sample table/data create kiya.

MySQL mein:

```sql
CREATE TABLE Products (
    ProductID CHAR(36) NOT NULL PRIMARY KEY,
    ProductName VARCHAR(255) NOT NULL,
    Category VARCHAR(100) NOT NULL,
    UnitPrice DECIMAL(18,2) NOT NULL,
    QuantityInStock INT NOT NULL
);
```

Sample data insert kiya, aur phir public endpoint ne HTTP 200 return kiya.

Successfully tested

```http
GET http://4.155.89.160/products
```

Response mein Laptop, Mouse, Keyboard, Monitor aur Office Chair aaye.

Ek caveat: ye table humne manually banayi thi, response ke fields ke basis par. Abhi ise actual EF Core model/migrations ke saath verify karna baaki hai.

### PART 11 — Kubernetes mein humare 8 Deployments aur 8 Services

Ye humara actual current deployment structure hai:

| Resource        | Deployment replicas | Service type | Purpose                 |
| --------------- | ------------------- | ------------ | ----------------------- |
| API Gateway     | 2                   | LoadBalancer | Public entry point      |
| User Service    | 2                   | ClusterIP    | Internal authentication |
| Product Service | 2                   | ClusterIP    | Internal product API    |
| Order Service   | 2                   | ClusterIP    | Internal order API      |
| MySQL           | 1                   | ClusterIP    | Product database        |
| MongoDB         | 1                   | ClusterIP    | Order database          |
| Redis           | 1                   | ClusterIP    | Cache                   |
| RabbitMQ        | 1                   | ClusterIP    | Message broker          |

Total:

### 8

Deployments

8 application/infrastructure workloads

### 8

Services

Stable networking endpoints

### 12

Desired Pods

2 + 2 + 2 + 2 + 1 + 1 + 1 + 1

### 1

Public entry point

API Gateway LoadBalancer

12 Pods ka calculation:

```text
API Gateway     = 2
User Service    = 2
Product Service = 2
Order Service   = 2
MySQL           = 1
MongoDB         = 1
Redis           = 1
RabbitMQ        = 1
-------------------
Total           = 12 Pods
```

Ye desired replica count hai. Runtime mein actual count temporarily differ kar sakta hai, for example rollout ke waqt ya kisi Pod failure ke waqt.

### PART 12 — Kubernetes commands jo humne use kiye aur unka meaning

Ye commands practically yaad rakhna. Interview mein bhi kaam aayenge.

### Resources dekhna

```cmd
kubectl get pods
```

Cluster ke Pods list karta hai.

```cmd
kubectl get deployments
```

Deployments list karta hai.

```cmd
kubectl get services
```

Services aur unke types, ClusterIP, External IP, ports dikhata hai.

```cmd
kubectl get all
```

Common resources ka overview deta hai. Lekin literally har Kubernetes resource nahi dikhata.

```cmd
kubectl get nodes
```

Worker Nodes ka status.

### Detailed inspection

```cmd
kubectl describe pod POD_NAME
```

Pod ki detailed information aur events.

```cmd
kubectl describe deployment product-service-deployment
```

Deployment configuration, ReplicaSets, events.

```cmd
kubectl describe service product-service
```

Service selector, ports aur endpoints related details.

### Logs

```cmd
kubectl logs POD_NAME
```

Pod ke container logs.

Deployment ke through:

```cmd
kubectl logs deployment/product-service-deployment
```

Specific container ho toh:

```cmd
kubectl logs POD_NAME -c CONTAINER_NAME
```

Live follow:

```cmd
kubectl logs -f POD_NAME
```

Previous crashed container ke logs:

```cmd
kubectl logs POD_NAME --previous
```

### Pod ke andar jaana

```cmd
kubectl exec -it POD_NAME -- sh
```

Container shell open karta hai, agar image mein `sh` available ho.

MySQL ke case mein humne use kiya:

```cmd
kubectl exec -it deployment/mysql-deployment -- mysql -uroot -p
```

### Port forwarding

Humne Product Service ko direct test karne ke liye:

```cmd
kubectl port-forward service/product-service 7121:8080
```

Meaning:

```text
Local machine port 7121
          |
          v
Kubernetes Service port 8080
```

Ab doosre CMD window mein:

```cmd
curl -i http://localhost:7121/api/products
```

Ye temporary testing tunnel hai. Iska matlab Service public internet par expose nahi hui.

### Apply and update

```cmd
kubectl apply -f deployment.yaml
```

YAML ke desired configuration ke according resource create/update karta hai.

```cmd
kubectl apply -f service.yaml
```

Service create/update karta hai.

```cmd
kubectl rollout status deployment/api-gateway-deployment
```

Deployment rollout complete hone ka status check karta hai.

```cmd
kubectl rollout history deployment/api-gateway-deployment
```

Deployment revision history dekhta hai.

```cmd
kubectl rollout undo deployment/api-gateway-deployment
```

Previous revision par rollback kar sakta hai, agar previous revision available ho.

### PART 13 — Humare current architecture mein kaunse issues aaye aur kyun?

Bhai, ye section interview mein sabse zyada valuable hai. Kyunki real-world engineer wahi hota hai jo issue debug kar sake.

| Issue                               | Root cause                                                      | Humara fix / status                  |
| ----------------------------------- | --------------------------------------------------------------- | ------------------------------------ |
| ImagePullBackOff type issue ka risk | AKS ko private ACR access chahiye                               | ACR attach kiya                      |
| Ocelot 502                          | Gateway localhost:7121 use kar raha tha                         | product-service:8080 use kiya        |
| Redis connection failure            | Redis Service available nahi thi                                | Redis Deployment + Service create ki |
| MySQL table missing                 | Database mein Products table nahi thi                           | Table aur sample rows create ki      |
| Build context error                 | Dockerfile ke relative paths ke hisaab se wrong folder se build | Parent directory se build kiya       |
| Gateway route missing for auth      | Ocelot mein Auth route configured nahi                          | Abhi pending                         |

### ImagePullBackOff kya hota hai?

Kubernetes image pull nahi kar paata.

Possible reasons:

* Image name wrong.

* Tag wrong.

* ACR permission missing.

* Registry authentication issue.

* Image actually exist nahi karti.

Debug:

```cmd
kubectl describe pod POD_NAME
```

Events mein exact error check kar.

### CrashLoopBackOff kya hota hai?

Container start hota hai, crash hota hai, Kubernetes restart karta hai, aur ye cycle repeat hoti hai.

Possible causes:

* Wrong connection string.

* Missing environment variable.

* Database unavailable.

* Application exception.

* Invalid configuration.

Debug:

```cmd
kubectl logs POD_NAME --previous
```

### Running aur Working mein difference

Ye bhi important:

`kubectl get pods` mein Pod `Running` dikha raha hai, iska matlab container start hua hai.

Iska ye matlab nahi ki:

* Database connected hai.

* Every endpoint working hai.

* RabbitMQ message successfully publish ho raha hai.

* Application fully healthy hai.

Application-level readiness aur integration testing alag se karni hoti hai.

### PART 14 — Humne kya complete kiya aur kya pending hai?

Main ye distinction clearly rakh raha hoon, kyunki interview mein jo actually kiya hai wahi bolna.

Done

Azure foundation

* Resource Group create kiya.

* ACR create kiya.

* AKS cluster create kiya.

* AKS ko ACR access diya.

* Local kubectl credentials configure kiye.

Done

Application deployment

* API Gateway, User, Product, Order deployments.

* MySQL, MongoDB, Redis, RabbitMQ deployments.

* Internal ClusterIP Services.

* API Gateway LoadBalancer Service.

* 2 replicas for each application microservice.

Verified

Product API

* Public IP working.

* Ocelot Product route fixed.

* Product Service internal DNS working.

* Redis dependency available on request path.

* MySQL table/data manually created.

* `GET /products` returns HTTP 200.

Pending

Remaining work

* PostgreSQL deployment for User Service.

* Auth routes in Ocelot.

* User registration/login end-to-end verification.

* Order API and MongoDB operations verification.

* RabbitMQ publish/consume end-to-end verification.

* Persistent volumes for databases.

* Liveness/readiness probes.

* CPU/memory requests and limits.

* HPA autoscaling.

* Production configuration, HTTPS and secure secrets.

* Monitoring and CI/CD.

Ek bahut important point: humne PostgreSQL abhi deploy nahi kiya tha, jabki User Service ko PostgreSQL chahiye. Isliye User Service ke Pods `Running` hone ke bawajood registration/login ko working claim nahi kar sakte.

Aur databases ke liye PVC configure nahi kiya. Iska matlab Pod replace hone par database ka data lose ho sakta hai. PVC ke bina ye setup learning/demo ke liye theek hai, lekin production ke liye nahi.

### PART 15 — Kubernetes ke kuch aur important concepts jo tujhe interview ke liye samajhne chahiye

Ye sab humare current deployment mein fully configure nahi kiye, lekin actual Kubernetes understanding ke liye important hain.

### 1\. Namespace

Namespace Kubernetes cluster ke andar resources ko logically separate karne ka mechanism hai.

Example:

```text
Kubernetes Cluster
    |
    +---- default
    |
    +---- development
    |
    +---- staging
    |
    +---- production
```

Maan le ek hi AKS cluster mein Development aur Production applications run karni hain.

Toh namespaces ke through resources logically isolate/organize kar sakte hain.

Create:

```cmd
kubectl create namespace development
```

Check:

```cmd
kubectl get namespaces
```

Specific namespace mein resources:

```cmd
kubectl get pods -n development
```

Humne abhi apne resources default namespace mein deploy kiye the.

### 2\. ConfigMap vs Secret

Dono configuration store karne ke liye hain.

ConfigMap: non-sensitive configuration.

Example:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: product-config
data:
  MYSQL_HOST: mysql
  REDIS_CONNECTION_STRING: redis:6379
```

Secret: sensitive values.

Example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mysql-secret
type: Opaque
stringData:
  MYSQL_ROOT_PASSWORD: my-password
```

Difference:

| ConfigMap                         | Secret                                                |
| --------------------------------- | ----------------------------------------------------- |
| Non-sensitive config              | Sensitive configuration                               |
| Hostnames, feature flags          | Passwords, tokens                                     |
| Plain text data representation    | Base64 representation by default                      |
| Sensitive credential ke liye nahi | Sensitive data ke liye, with proper security controls |

Humne MySQL aur RabbitMQ ke passwords Kubernetes Secrets mein rakhe the.

Production mein Azure Key Vault + appropriate AKS integration consider karna chahiye.

### 3\. Liveness Probe

Kubernetes ko check karna hai ki application alive hai ya nahi.

Agar application deadlock mein phas gayi aur process chal raha hai, toh liveness probe failure ke baad container restart ho sakta hai.

Example:

```yaml
livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 20
  periodSeconds: 10
```

Meaning:

* 20 seconds baad check start.

* Har 10 seconds check.

* `/health` endpoint se health verify.

### 4\. Readiness Probe

Readiness probe batata hai application traffic receive karne ke liye ready hai ya nahi.

Example:

```yaml
readinessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
```

Agar Pod ready nahi hai, toh Kubernetes Service us Pod ko normal traffic ke eligible endpoints se temporarily remove kar sakti hai.

Liveness vs Readiness:

* Liveness: Application ko restart karna zaroori hai kya?

* Readiness: Application abhi traffic receive karne ke liye ready hai kya?

### 5\. Resource Requests and Limits

Maan le humare Product Service ko CPU aur RAM chahiye.

Kubernetes ko resource requirements batane ke liye:

```yaml
resources:
  requests:
    cpu: "100m"
    memory: "128Mi"

  limits:
    cpu: "500m"
    memory: "512Mi"
```

Requests: scheduling ke waqt Kubernetes ko minimum requested resources batate hain.

Limits: container ke resource consumption par upper bound define karte hain.

Ye production deployment mein important hai, warna ek service excessive CPU/memory consume karke doosre workloads ko impact kar sakti hai.

### 6\. HPA — Horizontal Pod Autoscaler

Abhi humne Product Service ke 2 replicas manually define kiye.

Maan le traffic badh gaya aur humein 2 se 5 Pods chahiye.

HPA metrics ke basis par replicas automatically scale kar sakta hai.

Conceptual example:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler

metadata:
  name: product-service-hpa

spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: product-service-deployment

  minReplicas: 2
  maxReplicas: 5

  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

Is example mein:

* Minimum 2 Pods.

* Maximum 5 Pods.

* CPU utilization target 70%.

HPA use karne ke liye metrics pipeline, commonly Metrics Server, properly available hona chahiye.

Note: HPA Pod count scale karta hai. Agar cluster mein worker node capacity insufficient ho toh additional node capacity ke liye Cluster Autoscaler jaisi facility bhi relevant hoti hai.

### 7\. Persistent Volume (PV), PVC aur Storage

Abhi humare MySQL, MongoDB, Redis aur RabbitMQ Pods ke liye persistent volumes configure nahi hain.

Problem:

```text
Database Pod
    |
    v
Container writable layer
```

Agar Pod replace hua toh container filesystem ka data persist hone ki guarantee nahi hai.

Persistent storage ke liye:

```text
Pod
 |
 v
PVC (PersistentVolumeClaim)
 |
 v
PV (PersistentVolume)
 |
 v
Azure Managed Disk / Azure Storage
```

* PVC = application ki storage request.

* PV = actual provisioned storage resource.

* StorageClass = storage dynamically provision karne ke rules.

Azure mein Managed Disk ya suitable storage solution use kar sakte hain.

PVC object khud generally disk ka bill nahi hota; provisioned Azure storage resource ki capacity/type ke hisaab se charges lagte hain.

Production databases ke liye Azure managed database services bhi consider kar sakte hain, instead of self-hosting databases inside Kubernetes.

### 8\. Ingress

Abhi humne API Gateway ko directly LoadBalancer Service se expose kiya.

Ingress ek Kubernetes API resource hai jo HTTP/HTTPS routing rules define kar sakta hai, typically an Ingress Controller ke through.

Example concept:

```text
https://api.example.com/products
             |
             v
        Ingress
             |
             v
        API Gateway
             |
             v
       Product Service
```

Ingress khud automatically traffic handle nahi karta; uske liye compatible Ingress Controller chahiye.

Humare current setup mein Ingress configure nahi kiya hai.

### PART 16 — Interview mein sabse important questions aur answers

Ab main tujhe exact interview-style answers de raha hoon. Inhe samajhkar apne words mein bolna.

### Q1. What is Kubernetes?

"Kubernetes is an open-source container orchestration platform used to automate deployment, scaling, networking and management of containerized applications."

Example: humare eCommerce project mein Kubernetes multiple microservices ke Pods manage karta hai.

### Q2. What is AKS?

"Azure Kubernetes Service is Microsoft's managed Kubernetes service. Azure manages the Kubernetes control plane, while we deploy and manage our application workloads on the cluster."

### Q3. What is a Pod?

"A Pod is the smallest deployable unit in Kubernetes. It can contain one or more containers that share networking and storage resources."

Humare case mein each application Pod has one application container.

### Q4. What is a Deployment?

"Deployment is a Kubernetes resource used to manage application Pods through ReplicaSets. It supports replica management, rolling updates and rollbacks."

### Q5. What is a Service?

"A Kubernetes Service provides a stable network endpoint for a set of Pods. It allows applications to communicate without depending on changing Pod IP addresses."

### Q6. Why did you use ClusterIP?

"We used ClusterIP for internal communication between microservices and infrastructure components. For example, the API Gateway communicates with Product Service using the product-service DNS name."

### Q7. Why did you use LoadBalancer?

"We exposed our API Gateway using a LoadBalancer Service. In AKS, this integrates with Azure Load Balancer and provides an external IP address through which clients can access the application."

### Q8. How does one microservice communicate with another in Kubernetes?

"Each microservice has a Kubernetes Service. We use the Service DNS name instead of hardcoding Pod IP addresses. For example, our Order Service communicates with Product Service using `http://product-service:8080`."

### Q9. What happens if a Pod crashes?

"The Deployment manages a ReplicaSet, which continuously tries to maintain the desired number of Pods. If a Pod is lost, Kubernetes creates a replacement Pod."

### Q10. What is ACR and why did you use it?

"Azure Container Registry is a private registry where we store Docker images. We pushed our microservice images to ACR, configured AKS to access the registry, and referenced those images in our Kubernetes Deployment manifests."

### Q11. Difference between Docker Compose and Kubernetes?

"Docker Compose is useful for defining and running multi-container applications, especially in development. Kubernetes provides orchestration capabilities such as replica management, scheduling, self-healing, service discovery, rolling updates and scaling."

### Q12. How did you deploy your application to AKS?

"Our process was to build Docker images for each microservice, push them to Azure Container Registry, connect AKS with ACR, configure kubectl credentials, create Kubernetes Secrets, and deploy workloads using Deployment and Service YAML manifests. We exposed the API Gateway through a LoadBalancer Service and verified the Product API using the external IP."

### PART 17 — Ek baar poora process apne dimaag mein fit kar

Ye poori story hai jo humne practically ki:

### Our eCommerce AKS deployment pipeline

* Developed .NET Microservices

    API Gateway, User, Product and Order Services.

* Created Docker Images

    Dockerfiles se application ko container images mein package kiya.

* Pushed Images to ACR

    Images Azure Container Registry mein store ki.

* Created AKS Cluster

    Azure mein managed Kubernetes cluster provision kiya.

* Connected AKS to ACR

    Cluster ko private images pull karne ki permission di.

* Connected kubectl to AKS

    `az aks get-credentials` se local machine connect ki.

* Created Kubernetes Secrets

    Database aur RabbitMQ credentials configure kiye.

* Applied Deployment YAMLs

    Deployments, replicas, images, ports aur environment variables define kiye.

* Applied Service YAMLs

    Internal ClusterIP aur public LoadBalancer configure kiya.

* Configured Ocelot Routing

    Gateway se Product Service ke Kubernetes DNS endpoint par routing fix ki.

* Verified Public API

    `http://4.155.89.160/products` se successful HTTP 200 response verify kiya.

### PART 18 — Ab tera practical revision plan

Meri recommendation: ek hi din mein saare Kubernetes topics ratne ki zaroorat nahi. Is order mein master kar:

| Order | Topic                             | Goal                        |
| ----- | --------------------------------- | --------------------------- |
| 1     | Azure Resource Group, ACR, AKS    | Cloud foundation samajhna   |
| 2     | Control Plane, Worker Node        | Kubernetes architecture     |
| 3     | Pod, Deployment, ReplicaSet       | Workload management         |
| 4     | Deployment YAML                   | Har field explain kar paana |
| 5     | Service YAML                      | Selector, port, targetPort  |
| 6     | ClusterIP, NodePort, LoadBalancer | Networking                  |
| 7     | Kubernetes DNS                    | Microservice communication  |
| 8     | ConfigMap, Secret                 | Configuration management    |
| 9     | Probes, requests, limits          | Reliability                 |
| 10    | PVC, HPA, Ingress                 | Production readiness        |

### PART 19 — Chalo ab interview practice karte hain!

Ab main tujhe ek-ek question poochunga. Tu answer de. Main interviewer ki tarah check karunga, galat hua toh correct karunga aur follow-up bhi poochunga.

AKS Interview Quiz

1 / 5

Humare project mein API Gateway ko LoadBalancer aur Product Service ko ClusterIP kyun banaya?

Dono same hain, bas naming alag hai

Gateway ko external access chahiye, Product Service ko internal access

ClusterIP Pod create karta hai

LoadBalancer database ke liye mandatory hai

Check answer

Score: 0 / 5

Meri ek advice: sirf YAML ya commands yaad mat kar. Interviewer agar pooche "Agar Product Pod ka IP change ho gaya toh Gateway kaise communicate karega?" toh tu confidently explain kar sake ki humne Kubernetes Service DNS kyun use kiya.

Tera actual project abhi ek working AKS deployment ka foundation hai. Production-ready banane ke liye hum next phase mein PostgreSQL, persistent storage, probes, HPA, HTTPS, monitoring aur CI/CD complete karenge.
