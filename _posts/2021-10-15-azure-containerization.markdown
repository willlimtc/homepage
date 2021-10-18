---
layout: post
title:  "Will's Azure Magic Ingredient - Azure Migrate: App Containerization Tool"
date:   2021-10-15 16:52:00 +0000
categories: Azure Cloud-Native Azure-Kubernetes-Service ASP.NET 
---
  
Hello and happy fall! It's been a while since the last time I updated my blog. I am excited to share a recent project that I collaborated on with Azure Migrate engineering team at Microsoft. Without further ado, let's get started!

Although cloud native application has been gaining more acceptance and popularity across industries for the past few years, it is still a bumpy road for most of the organizations. As shown in the figure below, the journey to the cloud is a multi-step process that involves a lot of decision making process. For many organizations, it is technically and financially infeasible to rearchitect and rebuild all of their monolithic legacy applications without a huge amount of investment and disruption of their business. Luckily, it is commonly agreed that these legacy apps that are critical to the business oftentimes benefit from an enhanced lift-and-shit migration which is called cloud optimized apps as shown in the graph below. This approach includes deployment optimizations that enable key cloud services without changing the core architecture of the application. For example, you might containerize the application and deploy it into a container orchestrator, such as [Azure Kubernetes Service](https://azure.microsoft.com/en-us/services/kubernetes-service/#overview). Once in the cloud, the application can consume other cloud services such as databases, message queues, monitoring and distributed caching.

![Demo Picture]({{site.baseurl }}/assets/blogs/10152021_pic01.png)

You might think: wait a minute, I can directly migrate all of our on-prem legacy application to the Azure Kubernetes Service hosted on the cloud? How is that being done and does it also involve a significant amount of work? At Microsoft Ignite this year, the Azure Migrate engineering team announced the preview of the Azure Migrate: App Containerization tool to help organizations easily containerize and migrate apps to AKS. The App Containerization tool offers a point-and-containerize approach to repackage applications as containers with minimal to no code changes by using the running state of the application. The tool currently supports containerizing ASP.NET applications and Java web applications running on Apache Tomcat.

The App Containerization tool can help:

1. **Discover your application**: The tool remotely connects to the application servers running your ASP.NET application and discovers the application components. The tool creates a Dockerfile that can be used to create a container image for the application.
2. **Build the container image**: You can inspect and further customize the Dockerfile as per your application requirements and use that to build your application container image. The application container image is pushed to an Azure Container Registry you specify.
3. **Deploy to Azure Kubernetes Service**: The tool then generates the Kubernetes resource definition YAML files needed to deploy the containerized application to your Azure Kubernetes Service cluster. You can customize the YAML files and use them to deploy the application on AKS.

![Demo Picture]({{site.baseurl }}/assets/blogs/10152021_pic02.jpg)

Although not all applications can benefit from a straight shift to containers without significant rearchitecting, some of the benefits of moving existing apps to containers without rewriting include:

1. **Improved infrastructure utilization**: With containers, multiple applications can share resources and be hosted on the same infrastructure. This can help you consolidate infrastructure and improve utilization.
2. **Simplified management**: By hosting your applications on a modern managed platform like AKS and App Service, you can simplify your management practices. You can achieve this by retiring or reducing the infrastructure maintenance and management processes that you'd traditionally perform with owned infrastructure.
3. **Application portability**: With increased adoption and standardization of container specification formats and platforms, application portability is no longer a concern.
4. **Adopt modern management with DevOps**: Helps you adopt and standardize on modern practices for management and security and transition to DevOps.

Are you interested in trying it out? Here is the [official documentation of the App Containerization tool](https://docs.microsoft.com/en-us/azure/migrate/tutorial-app-containerization-aspnet-kubernetes). Also, my colleague Jason and I recently partnered with product engineering team and published a series of learn modules for the tool on the [Microsoft Learn website](https://docs.microsoft.com/en-us/learn/), which give you a great hands-on experience for testing out the tool. I list all the related module below and feel free to try it out! I am also happy to announce that at the beginning of this Financial Year at Microsoft, I have switched my focus from Azure application development to Microsoft business applications including [**Power Platform**](https://powerplatform.microsoft.com/en-us/). I will start another series in my blog called **Will-Power Today**. Stay tuned!

1. [Containerize and migrate ASP.NET applications to AKS](https://docs.microsoft.com/en-us/learn/modules/migrate-aspnet-app-azure-kubernetes-service/)
2. [Containerize and migrate ASP.NET applications to Azure App Service](https://docs.microsoft.com/en-us/learn/modules/migrate-aspnet-app-azure-app-service/)
3. [Containerize and migrate Java web applications to Azure Kubernetes Service](https://docs.microsoft.com/en-us/learn/modules/migrate-java-app-azure-kubernetes-service/)
4. [Containerize and migrate Java web applications to Azure App Service](https://docs.microsoft.com/en-us/learn/modules/migrate-java-app-azure-app-service/)
