---
layout: post
title:  "Will's Azure Magic Ingredient - Building Cloud Native Application 101 - PART I"
date:   2021-03-04 11:35:00 +0000
categories: Azure Cloud-Native Azure-Functions Cosmos-DB .NET-Core
---
  
Hello and happy March! In this blog, I am going to share a recent customer engagement experience on building cloud native scalable applications. To many IT practitioners, cloud native sounds like another industry buzzword and it is difficult to get started with some hands-on experience. Therefore, the customer engagement is designed for them to have such experience and a good starting point to build production systems that are more resilient, manageable and automated. The engagement went really well and our customer is very happy about moving towards the right direction. Since this engagement provided some common guidances for being more "cloud native" (on Azure, of course), I decided to post it in my blog to share it with larger audiences. Without further ado, let's get started!  

## The starting point 

We normally started customer engagement with a discovery session to access their current state and lay a common ground. We find one of their applications is a good starting point. The application is a very simple [ASP.NET](https://docs.microsoft.com/en-us/aspnet/overview) CRUD(Create, Read, Update and Delete) application to collect and manage certain types of information accompanying their business process. The user base is relatively small but the application is mission critical. The proposed architecture will add additional complexity to this simple application. These additional components are necessary to demonstrate how to build the cloud native application that achieves the following goals:

1. Microservice based
2. Automation and rapid iteration
3. Scalable and failure tolerant
4. Leveraging Platform Service

## The proposed architecture

The proposed architecture leverages the Azure cloud extensively, balancing security, availability, flexibility and cost. Also, the architecture is moving heavily towards the PaaS service as much as possible (compared to on-prem and IaaS) to simplify operations and reduce on going costs. The proposed architecture is shown below:

# Show the Pictures here

The proposed architecture employs a typical three-tier client-server pattern that separates the presentation, business logic and data persistence. Each layer will employ a set of Azure products and services to implement.

### Presentation layer

[Azure Traffic Manager](https://azure.microsoft.com/en-us/services/traffic-manager/)

Azure Traffic Manager is designed to support site load balancing, A/B Testing as well as to support fail-over routing.

[Azure App Services](https://docs.microsoft.com/en-us/azure/app-service/overview)

Azure App Services provides the hosting and scale points for the ASP.NET core web interface. Developers can take advantage of App Services “Deployment Slots” to demonstrate pilot deployments, beta test as well as their ability to support A/B testing.

[App Service Plan](https://docs.microsoft.com/en-us/azure/app-service/overview-hosting-plans)

App Service Plan is the logical container that runs the Azure App Service, which defines the region, number of VM instances as well as the pricing tier. In addition, Azure Function App (which will be introduced later) also has the option of running in the App Service Plan.

### Service layer

[API Management](https://docs.microsoft.com/en-us/azure/api-management/api-management-key-concepts)

API Management provides business services abstraction for the solution, which allows for versioning, authentication and monitoring of the services. It can also be used to provide services documentation if required.

[Azure Function App](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview)

Azure Function serves as the services hosting layer for solution, which eliminate the need for VMs or even Azure App Services hosting solution. Function app provides auto scaling, deployment slots and act as the container for most, if not all services in this solution. In this solution, we will leverage App Service Plan to host the Azure Function App, which enables the access to deployment slot capabilities.

### Persistence layer

[Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction)

Cosmos DB is the data persistence storage for the solution. The original application was developed to use SQL Server to support a relational structure. However, there are no business requirements for a relational database. Azure Cosmos DB can be a good alternative and provide the experience with a document centric persistence layer.

### Non-functional 

[Azure Monitor](https://docs.microsoft.com/en-us/azure/azure-monitor/overview)

Azure Monitor helps maximize the availability and performance of applications and services. It delivers a comprehensive solution for collecting, analyzing, and acting on telemetry from cloud and on-premises environments. This information helps understand how applications are performing and proactively identify issues affecting them and the resources they depend on.

[Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)

Application Insights, a feature of Azure Monitor, is an extensible Application Performance Management (APM) service for developers and DevOps professionals. It will automatically detect performance anomalies, and includes powerful analytics tools to help diagnose issues and to understand what users actually do with the application. It works for apps on a wide variety of platforms including .NET, Node.js, Java, and Python hosted on-premises, hybrid, or any public cloud. It integrates seamlessly with the DevOps practices, and has connection points to a variety of development tools.

## Implementation

The progresses of the workshops with our customers are usually dynamic, depending on a few variables including the length of the time, workshop settings(a remote setting which is the only option right now certainly poses more challenges), and of course the current skill sets that our customers have in terms of building cloud native applications. At the end of this particular engagement, out customer managed to implement a working solution that contains the three layers as shown in the architecture graph above, without integrating with Azure Traffic Manager, API Management and nonfunctional components. I will leave these components to the following blog posts in this cloud native application development series. So let's break down into these three layers and see how we helped our customers build this application.

### Persistence layer

Choosing where to start is more of a personal choice, especially for an application that is not complicated in this case. Therefore, I started building this application from the persistence layer. As mentioned earlier that this application is designed to leverage Cosmos DB, a type of NoSQL database. Compared to relational database, Cosmos DB has a few advantages in this scenario: first, Cosmos DB contains schema-agnostic data (JSON file in this case), whose schema is not dictated by the application; second, Cosmos DB is capable of handling large, unrelated, indeterminate or rapidly changing data, which can scale data horizontally by sharing across servers; last but not the least, Cosmos DB  is built to support multiple different models and thus APIs to access and manipulate data.

To spin up a Cosmos DB on Azure, there are a few ways to do it. In this workshop, we suggested our client to use the fundamental and easiest way - Azure portal. There are a few things to consider when creating the resource, which are listed below:

1. [Choice of API](https://docs.microsoft.com/en-us/azure/cosmos-db/): we used the SQL/Core API for this application, since its core functionality is the CRUD operation. Therefore, we can use the SQL-like query to get the data that meets certain criteria.

2. Consistency levels: the way that Cosmos DB achieves high availability and low latency is through a tradeoff. The core tradeoff is made through defining the consistency level, from strong all the way to eventual. Since we built the app just for the demonstration of regional use, the strong consistency level is used. If your application is scaled at global usage, it is strongly recommended to read the [documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/distribute-data-globally) to make the best choice.

3. [Partition key](https://docs.microsoft.com/en-us/azure/cosmos-db/partitioning-overview): once the Cosmos DB instance and the container have been created, it is important to specify the partition key to allow scale individual containers. Also the certain operations to the Cosmos DB enabled by SDKs require the partition key. Choosing the right partition key depends on the application, the [video](https://www.youtube.com/watch?v=KlXhee6R0rE) is a good resource to learn to choose the right partition key.

### Service Layer

Once we have the resource deployed in place, the next step is to crate the app that we have imagined. How do we choose the mobile development platform? The answer is really up to you, because there are always pros and cons for each mobile development platform. In this case, I chose [Xamarin.Forms](https://dotnet.microsoft.com/apps/xamarin/xamarin-forms), which is an open source cross platform framework from Microsoft for building iOS, Android and Windows apps with .NET from a single share code base. The benefit also extends to the Surface DUO device, as the DUO provides [a SDK with Xamarin](https://docs.microsoft.com/en-us/dual-screen/xamarin/use-sdk) to enable the coordination between the two screens.  

What about the Azure Communication Service SDKs? Well, here is the complete [list of client libraries ACS has provided](https://docs.microsoft.com/en-us/azure/communication-services/concepts/sdk-options). Unfortunately, it hasn't provided Xamarin supportability yet. Luckily, we have a huge developer community that support us. Here's one [project](https://github.com/Laerdal/Xamarin.AzureCommunicationCalling) that encapsulate the JAVA library into the Xamarin project.  

With the platform and SDKs in place, now we can develop the sample app. A few critical things to tackle during the development is listed here:  

1. [Create and manage ACS access tokens](https://docs.microsoft.com/en-us/azure/communication-services/quickstarts/access-tokens?pivots=programming-language-csharp). You need to create either a HTTP-based or in-place user access token management service.
2. [Make phone call using ACS](https://docs.microsoft.com/en-us/azure/communication-services/quickstarts/voice-video-calling/pstn-call?pivots=platform-android). One core function of the app and a phone number on Azure is required.
3. [Enable video chatting on ACS](https://docs.microsoft.com/en-us/azure/communication-services/quickstarts/voice-video-calling/getting-started-with-calling?pivots=platform-android). A critical function to help the customers.
4. [Xamarin two pane view](https://docs.microsoft.com/en-us/dual-screen/xamarin/twopaneview). A NuGet package to help dictate how two screens present the app layout.
5. [Xamarin Data Binding](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/data-binding/). Linking the data from two layouts or screens.

### So how the app looks like now

Alright, below is the demo app at the moment. It can do the following tasks:

- Making a phone call: you can input the phone number in the text box and make a phone call to any U.S. based numbers.
- Making a group video chat: you can join a group video chat that all the participants share the same conference ID token.
- Viewing and Editing ticket information: you can view and edit the ticket information while calling or video chatting with your customer. The viewing pane can be customized into anything that suits your business needs!

![Demo Picture]({{site.baseurl }}/assets/blogs/12032020_pic01.jpg)

## Acknowledgement and next steps

Huge thanks to the open source developer community around Microsoft product and services. The [AzureCommunicationCalling](https://github.com/Laerdal/Xamarin.AzureCommunicationCalling) project lays out a great foundation for this demo. I also greatly appreciate the support from the very knowledgeable veteran technical architect Todd Van Nurden in MTC Minneapolis (I enjoyed your recount of the history of Xamarin Forms a lot!). Last but not least, kudos to my manager Ali Mazaheri for seeing the potential magic reactions between ACS & DUO and pointing out the direction.  

For the next steps, the code will be made available to the GitHub repo soon. As you may also see, there are a whole lot of features that we can leverage Azure services and build upon the current demo. To name a few:

- A "fancier" UI: you certainly have the same conclusion after seeing the demo picture lol.
- User AuthN & AuthZ: Azure Active Directory provides a comprehensive enterprise identity service features B2C that can perfectly apply to this scenario, especially you want to build a HTTP-based ACS access token service.
- NoSQL backend: Azure CosmosDB provides a simply to use and security compliant solution to store the ticket information on the backend. 

Stay tuned and let me know if you have any good ideas on this!

