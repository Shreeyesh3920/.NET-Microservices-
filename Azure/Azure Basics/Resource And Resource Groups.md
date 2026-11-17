## **1. What is an Azure Resource?**

A **resource is an individual service or component that you create in Azure to build your application.**

For example, in your e-commerce project, you have:

- Product Service – .NET Web API
- Order Service – .NET Web API
- User Service – .NET Web API
- MySQL database
- MongoDB database
- Redis cache

In Azure, you can create resources to host or manage these components.

Examples:

| Azure resource         | Purpose                        |
| ---------------------- | ------------------------------ |
| App Service            | Host your .NET Web API         |
| Azure SQL Database     | Store application data         |
| Storage Account        | Store files and documents      |
| Azure Cache for Redis  | Cache frequently accessed data |
| Key Vault              | Store secrets and credentials  |
| Virtual Network (VNet) | Create a private network       |

Think of a resource as an individual building block of your application.

## **2. What is a Resource Group?**

A **Resource Group is a logical container that holds related Azure resources.**
<img width="972" height="523" alt="image" src="https://github.com/user-attachments/assets/9714be4a-8ba6-483e-b225-a6bedefc5954" />

Imagine you're building your e-commerce application. You might create:

<img width="988" height="487" alt="image" src="https://github.com/user-attachments/assets/f237a7cb-a5d7-4b89-a5f9-6556aef357fe" />

Here:

- `ECommerce-RG` is the Resource Group.
- Everything inside it is an individual Azure Resource.
- You can manage related resources together.

## **3. Why do we need Resource Groups?**

Three important reasons:

1. **Organization:** Keep resources belonging to your e-commerce application together.
2. **Management:** View, manage, and monitor related resources from one place.
3. **Deletion:** If you delete a Resource Group, Azure deletes the resources inside it too. Be careful!

<img width="997" height="583" alt="image" src="https://github.com/user-attachments/assets/02364d55-1d3a-4315-b6b0-d634cb44ae7a" />

**Remember this:**

- Subscription = Your Azure account's resource and billing boundary.
- Resource Group = Container for organizing related resources.
- Resource = An individual Azure service.

