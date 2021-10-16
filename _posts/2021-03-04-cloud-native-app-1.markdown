---
layout: post
title:  "Will's Azure Magic Ingredient - Building Cloud Native Application 101"
date:   2021-03-04 11:35:00 +0000
categories: Azure Cloud-Native Azure-Functions Cosmos-DB .NET-Core
---
  
Hello and happy March! In this blog, I am going to share a recent customer engagement experience on building cloud native scalable applications. To many IT practitioners, cloud native sounds like another industry buzzword and it is difficult to get started with some hands-on experience. Therefore, the customer engagement is designed for them to have such experience and a good starting point to build production systems that are more resilient, manageable and automated. The engagement went really well and our customer is very happy about moving towards the right direction. Since this engagement provided some common guidances for being more "cloud native" (on Azure, of course), I decided to post it in my blog to share it with larger audiences. Without further ado, let's get started!  

## The starting point 

We normally started customer engagement with a discovery session to access their current state and lay a common ground. We find one of their applications is a good starting point. The application is a very simple [ASP.NET](https://docs.microsoft.com/en-us/aspnet/overview) CRUD(Create, Read, Update and Delete) application to collect and manage certain types of information accompanying their business process. The user base is relatively small but the application is mission critical. The proposed architecture will add additional complexity to this simple application. These additional components are necessary to demonstrate how to build the cloud native application that achieves the following goals:

1. Microservice based
2. Automation and rapid iteration
3. Scalable and failure tolerant
4. Leveraging Platform-as-a-Service(PaaS)

## The proposed architecture

The proposed architecture leverages the Azure cloud extensively, balancing security, availability, flexibility and cost. Also, the architecture is moving heavily towards the PaaS service as much as possible (compared to on-prem and IaaS) to simplify operations and reduce on going costs. The proposed architecture is shown below:

![Demo Picture]({{site.baseurl }}/assets/blogs/03042021_pic02.png)

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

Azure Function serves as the services hosting layer for solution, which eliminate the need for VMs or even Azure App Services hosting solution. Function app provides auto scaling, deployment slots and act as the container for most, if not all services in this solution. It is also worth noting that  App Service Plan can host the Azure Function App, which enables the access to deployment slot capabilities.

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

The service layer, also known as the logic tier or middle tier, is the heart of the application. In this tier, the requests from the presentation layer are processed - in this case against all the information stored in the data persistence layer, as the core function of this application is to create, read, update and delete data entries (in this case, recruit information) in the Cosmos DB. In order to implement the service layer, there are a few things to consider:

- Abstraction of the service endpoint to the presentation layer as RESTful APIs
- Managing connections to the Cosmos DB in the persistence layer
- Can handle the increase of the demand from the persistence layer through automatic scaling

With these things in mind, we chose implementing service layer using the Azure Functions, as it provides an elegant solution to all of the issue above. First of all, Azure Function provides a wide variety of execution triggers including the HTTP triggers with customized routing scheme. Therefore, Azure Functions can be exposed as RESTFul APIs, which abstract the business logics and are easy to be managed by platforms such as Azure API Management. Azure Functions also provide a great feature called input/output bindings that can soothly manage the connections to the Cosmos DB. Finally, as a serverless solution that is event driven, Azure Functions can scale automatically as demand increase and you only pay for the number of executions of the Functions (note: may vary depending on the plan you choose).

To create the Azure Function App, a simple way is through Visual Studio, as it is now fully integrated to the Azure application development practices. When creating a project, you can choose Function app with the language of your choice and then Visual Studio can automatically set up the the default environment for you. To create the CRUD APIs for the presentation layer, we follow the best practice of APIs design and list the APIs for the Azure functions as below (recruit is the entity for the operations):

    HTTP POST    /recruit       //Create a recruit entry
    HTTP GET     /recruit       //List all the recruit entries
    HTTP GET     /recruit/id    //Get the recruit entry with the id
    HTTP PUT     /recruit/id    //Update the recruit entry with the id
    HTTP DELETE  /recruit/id    //Delete the recruit entry with the id

The sample code for the create Azure Function is listed below:

        public static void CreateRecruit(
            [HttpTrigger(AuthorizationLevel.Function, "post", Route = "recruit")] HttpRequest req,
            [CosmosDB (
                databaseName:"recruit-db",
                collectionName:"recruits",
                ConnectionStringSetting = "CosmosDBConnection")] out dynamic document,
                ILogger log)
        {
            log.LogInformation("Crate a recruit entry.");

            string requestBody = new StreamReader(req.Body).ReadToEnd();
            var input = JsonConvert.DeserializeObject<Recruit>(requestBody);
            document = input;
        }

### Identity management 

Like any software applications, cloud native applications need to have some knowledge of the user or service that is calling them. The user or service interacting with an application is known as a security principal, and the process of authenticating and authorizing these principals is known as identity management. Authentication is the process of determining the identity of a security principal. Authorization is the act of granting an authenticated principal permission to perform an action or access a resource. Sometimes authentication is shortened to `AuthN` and authorization is shortened to `AuthZ`. Cloud-native applications need to rely on open HTTP-based protocols to authenticate security principals since both clients and applications could be running anywhere in the world on any platform or device. 

[Microsoft Azure Active Directory (Azure AD)](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-whatis) offers identity and access management as a service. Customers use it to configure and maintain who users are, what information to store about them, who can access that information, who can manage it, and what apps can access it. AAD can authenticate users for applications configured to use it, providing a single sign-on (SSO) experience. Due to the time limit for this workshop, we haven't implemented the Azure AD integration with the application, which will be explained in greater details in the following blogs.

### Presentation layer

The presentation layer is the user interface and communication layer of the application, where the end user interacts with the application. Its main purpose is to display information and collect information from the user. Since this application is designed as web application, the things needed to consider for implementing the application include the web develop framework and implementation details such as pagination.

#### The framework

There are tons of web development framework in the market including ExpressJS, Django and React.js etc. There are alway pros and cons of selecting one over another. In this workshop, we chose the [ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-5.0) MVC to implement the presentation layer, as it seamlessly integrates into the Azure App Service that enable publish the application by clicking only one button. If you are still a bit confused about the .NET ecosystem, this [article](https://www.azurebarry.com/dot-net-ecosystem-explained/) gives a good explanation. 

#### Pagination

One of the most important implementation details of this CRUD application is pagination, meaning that user can request a page number (or plus the page size), the web application will return exactly what has been request. Since this application is designed for handling millions of records, pagination will help reduce the service load as well as improve the system security. There are two parts for implementing pagination: the RESTFul API endpoint side on the service layer and the controller side on the presentation layer. On the API endpoint side, it is required to respond with limit number of records and can retrieve the records at a certain point from the Cosmos DB. To meet these two requirements, we leveraged a feature in the Cosmos DB SDK called `ContinuationToken`, which is a static breakpoint from which the SDK can read records from Cosmos DB. This [article](https://www.chasingdevops.com/paging-azure-cosmos-db-sql-net-sdk/) gives a detailed explanation of how to use this feature in the RESTFul API endpoint. In this application, we implement the `ContinuationToken` in the Function that corresponds to the `GET /recruit` API, since Function is the data access layer. As shown in the code below, the Function takes the HTTP request and read through the request body to see if any token is included. Then the `FeedOptions` in the Cosmos SDK encapsulates the these options and queries the Cosmos DB to get the desired results. The result itself also contains information on if there are more results yet to be retrieved.

        [FunctionName("hfdr-list")]
        public static async Task<IActionResult> ListRecruit(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = "recruit")] HttpRequest req,
            [CosmosDB(
                databaseName:"recruit-db",
                collectionName:"recruits",
                ConnectionStringSetting = "CosmosDBConnection"
                )] DocumentClient client, 
            ILogger log)
        {
            log.LogInformation("Get the all the recruit entries, with continual token");
            var queryParams = req.GetQueryParameterDictionary();
            var count = Int32.Parse(queryParams.FirstOrDefault(q => q.Key == "count").Value ?? "-1");

            string pToken = await new StreamReader(req.Body).ReadToEndAsync();
           
            var feedOptions = new FeedOptions()
            {
                MaxItemCount = count,
                RequestContinuation = pToken
            };

            var uri = UriFactory.CreateDocumentCollectionUri("recruit-db", "recruits");
            var query = client.CreateDocumentQuery(uri, feedOptions).AsDocumentQuery();
            var results = await query.ExecuteNextAsync();

            return new OkObjectResult(new
            {
                hasMoreResults = query.HasMoreResults,
                pagingToken = query.HasMoreResults ? results.ResponseContinuation : null,
                results = results.ToList()
            });
        }

 On the controller side, we designed a mechanism to manage the page number display and the `ContinuationToken` through using HTTP Session. The fundamental philosophy is to cache the ContinuationToken for a specific page when it has been obtained, so that we don't need to retrieve the token when the page is visited again. In terms of implementation, the first thing is to enable a `SessionExtensions` required by ASP.NET MVC to mandate data conversion during the `Get` and `Set` operation:

     public static class SessionExtensions
    {
        public static void Set<T>(this ISession session, string key, T value)
        {
            session.SetString(key, JsonConvert.SerializeObject(value));
        }

        public static T Get<T>(this ISession session, string key)
        {
            var value = session.GetString(key);
            return value == null ? default(T) : JsonConvert.DeserializeObject<T>(value);
        }
    }

Then a session data is maintained as a dictionary mapping the page number to the corresponding `ContinuationToken`. Therefore whenever a request for a certain page comes, the following code will find the nearest preceding page with `ContinuationToken` and start to make request to the API to get the paginated result while caching the paging token along the way. Therefore, the current session will likely keep all the tokens after the user clicking a few pages.

        public async Task<List<Recruit>> PaginatedResult(int currentPage, int totalPage)
        {
            @ViewBag.CurrentPage = currentPage;
            @ViewBag.TotalPage = totalPage;
            PagedRecruit pRecruit;
            var dict = HttpContext.Session.Get<Dictionary<int, string>>("currDict");
            if (dict == null) dict = new Dictionary<int, string>();
            int pageWithToken = currentPage;
            string cToken = null;

            while(pageWithToken > 1)
            {
                if(dict.ContainsKey(pageWithToken))
                {
                    cToken = dict[pageWithToken];
                    break;
                }
                pageWithToken--;

            }

            bool samePage = pageWithToken == currentPage;
            string queryString = Configuration["ConnectionStrings:ListPageResult"] + "&count=" + PAGE_SIZE;

            do
            {
                using (HttpClient client = new HttpClient())
                {
                    var httpReq = new HttpRequestMessage
                    {
                        Method = HttpMethod.Get,
                        RequestUri = new Uri(queryString),
                        Content = cToken == null ? null : new StringContent(cToken, Encoding.UTF8)
                    };

                    using (HttpResponseMessage res = await client.SendAsync(httpReq).ConfigureAwait(false))
                    {
                        using (HttpContent content = res.Content)
                        {
                            var jsonResult = await content.ReadAsStringAsync();
                            pRecruit = JsonConvert.DeserializeObject<PagedRecruit>(jsonResult);
                            if(pRecruit.HasMoreResults)
                            {
                                cToken = pRecruit.PagingToken;
                                if(!dict.ContainsKey(pageWithToken + 1))
                                    dict.Add(pageWithToken + 1, cToken);
                            }
                        }
                    }
                }
                pageWithToken++;
            } while (pageWithToken <= currentPage && (!samePage));

            HttpContext.Session.Set<Dictionary<int, string>>("currDict", dict);
            return pRecruit.Results;
        }



## Summary, acknowledgement and next steps

So, with implementing the basic function of the three-layer application, our customer has made the first step towards building cloud native application in their production system. I have uploaded my sample code on [github](https://github.com/graphene-azure-app/hfdr-application), and feel free to reach out to me if you have any questions.

I greatly appreciate the support from very knowledgeable compatriot technical architects Todd Van Nurden in MTC Minneapolis, Paul Fisher in MTC Reston and Ravi Tella in MTC Houston, especially Todd who spent a lot of time to discuss a better solution with me. Like he said during the engagement, everyone is learning during the workshop, including the Microsoft side. It is the growth mindset that has made Microsoft better know our customers and serve their needs.

This is just a very simplified version of cloud native application, and there are a whole lot of features that we can leverage Azure services and build upon the current demo. To name a few:

- Better security: this sample code hasn't implemented any security features including authentication & authorization, and secret management. No worries, Azure Active Directory and Azure Key Vault are here to help! 
- Better management: managing application is important when put into the production, and using Azure Traffic Manager, API Management, Azure Monitor and Application Insights can offer a comprehensive way to manage the application on every layer and through different lens.
- Better practice: to enable continual improvement of the application and therefore continual deliver values to the end user of the application, it is essential to integrate DevOps into the development and deployment operations for the application. Azure Dev Ops and GitHub are two great tools/platforms to enable DevOps.

Stay tuned for the part Deux of this series!

