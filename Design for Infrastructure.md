## **Migrating Solutions**

If you need complete access to the underlying infrastructure: make use of VMs, IaaS
When we dont need that, we can migrate to Azure Web Apps, using PaaS. Careful to make sure that the app programming language is available.
***Azure VM vs Azure Web App***
![[Pasted image 20240911173849.png]]

### Deployment Slots (available on the Az Web App)

***Staging Environments for App Service Plans.*** 

Create a staging deployment slot within the web app when we have a newer version of the app.
Deploy the new version there and test whether there are any issues. Then we can just directly swap the staging slot with the production slot, eliminating the downtime for your app when new changes are deployed. 
Apps in deployment slots have their own host names

### Azure Batch 

Used to run large-scale, parallel and high performance computing batch ***jobs*** on pools of virtual machines.

Manages the tasks and jobs that need to be run on virtual machines (pools) for processing the data. 

Quota limitations for VM pools creations are in place!!  

### CONTAINERS

In the case where we have multiple apps running in the same VM, there may be multiple issues that arise regarding dependencies. For example we install third-party libraries that we need for one app, and it causes issues in the other app; dependency issues.
![[Pasted image 20240912103150.png]] *one vm, multiple apps installed*

In order to deal with this, we can use containers, which help to **package** the app along with the libraries, frameworks and dependencies that are required for that app; and make it isolated from other apps, which will be in other containers.
![[Pasted image 20240912103520.png]] 
*using containers, apps run in isolation*

***>>If we are running our app on a VM***, we should install the Docker toolset in it in order to continue with the creation of containers etc. For containers, we need to have [[images]] in place, with info on how to create the container, with the app as well as underlying libraries frameworks etc. To store the container we can either use Docker Hub or the Azure Container Registry.
**Docker HUB**
Here we can find template images (for example a mySQL image) on top of which we can add our custom settings, using a dockerfile. Using a tool named WinSCP, we can directly copy any files we need from our machine onto our VM, so the dockerfile and our sql file in this case.

We can use the docker build command, to build a new custom image with the name "appsqlimage". This image will be built on top of mySQL, since we have specified that in the dockerfile, and it will contain our sql file as well.
`sudo docker build -t appsqlimage`

the dockerfile could look like this: 
```docker
FROM mysql as base

ENV MYSQL_ROOT_PASSWORD=Azure123
ENV MYSQL_DATABASE=appdb

COPY 01.sql /docker-entrypoint-initdb.d/
```

Then we can either post the created image on the Docker Hub, or we can create a Container Registry in Azure and push it there

 **Choosing Between Docker Hub and Azure Container Registry:**
- ***If you need global accessibility*** and a widely known registry, **Docker Hub** might be suitable.
- ***If you need Azure-specific features***, better performance with Azure services, or better control over security and compliance, then **Azure Container Registry** is a better choice.

---

***>>>Using the Azure Container Instance***
Azure Container Groups can have multiple containers. Offers simple deployment of containers. 
No need to have a VM in place, all the underlying infrastructure is managed for you.
**Container Restart Policy** - Always and OnFailure, to determine when will the containers be restarted, as explained by the names
The moment when a container is restarted, data in it is lost. In a Az Container Instance, we can persist our data to an Azure file share, so no matter what happens with our container (gets restarted), our data is safe

---

***>>>Azure Kubernetes***
Deployment of multiple container-based applications. Orchestration of container-based apps.
Create Kubernetes cluster, you can integrate it with an existing Azure Container Registry.

### Microservices

**In Monolithic applications**, there is one app and different modules with different functionalities which are dependent on one another; if we were to update one of the modules, we would have to adjust the whole app and redeploy it as a whole. 
![[Pasted image 20240912105116.png]]
While in a ***microservices*** architecture, each functionality is wrapped in a service, independent from all the other services. So in case of a change in one of the services, we would only update and worry about that single service working, and the rest of the services would be unaffected; no need to redeploy the whole app, only that service. 
Services can even be built in different technologies and languages
![[Pasted image 20240912105505.png]]

### Azure File Sync

Syncs up "Azure File Shares" with your on-premises file-servers, using Azure File Sync Agent

Big pro: we have a backup of the file shares, in case of any server failure

Create sync groups and include the Azure File Share and our file server.




## **Networking Solutions**

### VPN Connections

***Azure Virtual Network Gateway*** - Service
- **Purpose:** Enables encrypted traffic over the public internet using VPN protocols (such as IPsec/IKE). It is used for establishing secure VPN connections.
- **Types of Connections Supported:**
    - **Site-to-Site (S2S) VPN:** Connects an entire on-premises network to an Azure VNet over the internet (create a gateway subnet for this).
    - **Point-to-Site (P2S) VPN:** Connects individual clients (such as remote users or devices) to an Azure VNet over the internet.
    - **VNet-to-VNet VPN:** Connects two or more Azure VNets, even if they are in different Azure regions.
- **Use Case:** Suitable for securely extending on-premises networks to Azure, connecting remote clients, or linking multiple VNets.
---
***Azure ExpressRoute***
- **Purpose:** Enables private, dedicated connections between an on-premises network and Azure, bypassing the public internet. ExpressRoute connections provide higher reliability, lower latency, and higher security compared to regular internet connections.
- **Types of Connections Supported:**
    - **ExpressRoute Circuit:** A private connection between Azure and on-premises networks provided by a connectivity provider. It does not use the public internet.
- **Use Case:** Suitable for organizations that require secure, private connections with predictable network performance (e.g., for financial services, healthcare, or any industry that needs high security and reliability).
- ---
***Azure Bastion Subnet***
- **Site-to-Site VPN**

---

***Virtual WAN***
When we have multiple networks and there will need to be multiple gateways configured it can get complex. Creating a virtual WAN would be helpful.

### Network Watcher service

***Tools:***
- Connection Monitor
- Next hop
- IP Flow Verify
- Connection troubleshoot
- NSG Diagnostic
- NSG Flow Logs
- Traffic Analysis

### Azure Firewall service

Used to connect a whole network group.
Has in built traffic threat-detection
You can create network rules and application firewall rules as well

### Service Endpoints

Services are publicly accessible through the internet, which makes them vulnerable to it. By using Service Endpoints, it builds a secure connection between our virtual network and its services, over the Azure backbone network.
Then we restrict the access into our service (in the networking section, choose "Allow access from **Selected Networks**", to make it only accessible through our endpoint.

### Azure Load Balancer*

A load balancer is a network device or software that distributes incoming network traffic across multiple servers or resources to ensure that no single server becomes overwhelmed.

--> We can connect a ***PUBLIC*** Load Balancer into our Azure virtual network, treating all our VMs in this virtual network as a backend pool and distributing all incoming traffic among them. So the requests will firstly go through our Load Balancer, meaning we can assign a single public IP address to it, and not our VMs.

Another thing we can use is a **Health Probe**, which makes sure that our virtual machines are healthy and running, as it does not make sense for the load balancer to route request into a machine that is not working. 
We can define load balancing rules that determine how the requests are taken from the users and distributed across the machines in the backend pool.

--> We can also use an ***INTERNAL*** Load Balancer. This is useful when we need to access for ex. a database which is in a private network, not exposed to the internet, app making requests to the load balancer, which then sends the req to the private subnet. 
Achieved via Availability Groups, initially the Load Balancer sends all the requests to the primary VM, and then in case of a failover it sends them to the secondary.

Lets the VMs not have a public IP address, enhancing security.

### Azure Bastion Connection
Azure Bastion provides a highly secure, fully managed, and scalable solution for accessing Azure VMs **remotely**. It eliminates the risks associated with exposing VMs to the public internet and simplifies secure access management through the Azure portal.

***How Azure Bastion Works:***
	- **Deployment**: You deploy an Azure Bastion host to the virtual network where your VMs are located. The Bastion host is then used to manage RDP or SSH connections to your VMs over a secure channel.
	- **Access**: When a user wants to connect to a VM, they initiate a connection through the Azure portal using Bastion. The connection is established securely over HTTPS (port 443) to the Azure Bastion host, which then forwards the connection to the target VM within the private network.

***Use Cases:***
	- **Secure Remote Management**: For organizations that need to securely manage VMs without exposing them to the public internet.
	- **DevOps and IT Operations**: Useful for administrators, developers, and IT teams who need secure, centralized access to Azure VMs for management, troubleshooting, and maintenance tasks.
	- **Compliance and Security**: Suitable for scenarios where strict compliance and security requirements mandate minimal exposure to the public internet.

### Azure Application Gateway*

Needs a VN subnet where to host its resources
- Understands the HTTP request being made and distributes accordingly (works at layer 7, thats why it understands the req)
- Web Application Firewall works in addition
**URL routing**
The **URL routing** feature in Azure Application Gateway enables you to direct traffic based on URL paths and hostnames to different backend pools, improving the flexibility, performance, and security of your web applications. It's particularly useful in scenarios where you need to manage multiple sites, services, or microservices architectures within a single application gateway instance.

---
***WEB Application Firewall***
Builds on top of the App Gateway and protects apps against common attacks like SQL injection and cross-site scripting. It offers Detection as well as *Prevention* of these attacks. 

---
***Azure Traffic Manager***
DNS based traffic load balancer. Used to distribute traffic to public facing apps across different Azure *regions*. WHEN WE HAVE APPS IN DIFFERENT REGIONS
Priority Routing Method -> add endpoints for each web app with a specific priority (priority=1 -> most prioritized)

### Azure CDN Service (& Front Door)*
	***CDN - Content Delivery Networking***
Azure CDN is a globally distributed network of servers (edge locations) that cache and deliver web content, such as images, videos, stylesheets, scripts, and other static assets, closer to the users to reduce latency and improve performance.
Helps to deliver content to users across the globe by placing content on physical nodes placed across the world. Create a CDN Profile on a Global level. 
**Caching of *Static* Content**
Caches responses to the servers closest to users, so in case of two same requests, it gets its next response from the server closest, making it seamless. Request first goes through the global level CDN profile endpoint, which determines this cache work.

***Azure Front Door***
Azure Front Door is a global, scalable, and secure entry point for fast delivery of your web applications. It is a **layer 7 (HTTP/HTTPS) load balancer and application acceleration service** that provides intelligent traffic routing, load balancing, and enhanced security features. ***Front door is used to ensure that your request goes on to a web endpoint that has the lowest latency.***

***Choosing Between Azure Front Door and Azure CDN:***
- **Use Azure Front Door** if you need:
    - Global load balancing and intelligent traffic routing.
    - Application acceleration for both static and dynamic content.
    - A web application firewall (WAF) for security.
    - A single, unified entry point for a globally distributed web application.
    - Advanced routing features, such as path-based or host-based routing.
- **Use Azure CDN** if you need:
    - To accelerate the delivery of static content like images, videos, and files to users globally.
    - To reduce latency by caching content at multiple edge locations.
    - To offload traffic from your origin servers to save costs and improve performance.
    - A simple, cost-effective solution for delivering large amounts of content with minimal configuration.
### Azure Cache for Redis

Caching service, to save most frequently needed datasets. We need to pre-cache these datasets beforehand (for example once a day), so the fetching from the db is only done once and saved in the cache 

## **Development Solutions**

### Azure Event Hubs
Event Hub Namespace
A Big-Data streaming platform, receiving and processing millions of events per second.
![[Pasted image 20240912170934.png]]

We can stream log info from different services into the azure event hub

You can also capture this data to a blob storage.
### Azure Functions

A serverless solution, the entire compute infrastructure is managed for you by Azure. A great solution for processing bulk data, integrating systems working with IoT and building simple APIs and micro services.

Supported languages: c#, java, js, python, poweshell

Pay-per-use pricing model --> You are only charged based on how much its fired

Bring your own dependencies

Integrated Security

Steps in portal: 
1. create Function App -> 
2. create functions within the function app, 
3. choose among the different trigger options like:
	 - on HTTP request, 
	 - on scheduled timer, 
	 - on Document Addition or modification in Azure Cosmos DB etc.

### Azure Service Bus

Messaging service

Allow different components of an app to send messages to each other. Decouples components

You can create queues, where to send and receive messages.

You can create topics and send a message to a topic, which is then sent out to multiple users which have subscribed to that topic

A scenario to be used together with Azure functions:

- We have an initial app which sends objects to an azure storage account, into a container named "Unprocessed". 
- With each object sent, a message is also sent into the service bus queue, which contains information about the unprocessed object inside the container.
- Then an Azure function app is used to read the message from the queue and based on its contents, process the object accordingly and move it to the "Processed" container. The azure function will be based on a "Service Bus trigger"

### Azure Logic Apps

Provides a cloud platform where you can create and run automated workflows

You can set a trigger to the workflow, it has connections with different services triggers (such as a blob being created for example) or other Cloud Systems  as well

You can set various actions in the workflow

### Azure Event Grid


### Azure API Management Service

Abstract the implementation of the backend API's 
Allow customers to securely access backend API's

Instead of users and apps calling the API directly, they can call it through the Azure api management working as an extra layer. It offers a few features such as:
for added security; for limiting the rates and number of calls to the api so as to not overflood it etc

In order to invoke the api through the endpoint, there needs to be a **subscription key** in place, added security.

**Azure DevOps set of tools**
--
**Azure DevOps account** type exists. It has a different interface from a regular azure portal user. 

*DevOps lifecycle*
![[Pasted image 20240913142634.png]]

A way to make the work between the development team and operations team more cooperative.

You create a project in your devops organization. There are multiple devops setup tools you can access:
- Boards 
	- add tasks
	- add user stories
	- add into backlogs and sprints
- Repos - host your codes via git repos
	- just like github, to have your repos in the cloud
	- 
- Pipelines - take our code and build it 
	- uses a yaml based file for the pipeline (yaml pipeline)
	1. Pipeline to build as well as publish if we wish
		- you can also publish it as an artifact
		- it adds the yaml pipeline file to the repository
		- running this yaml file will create a "job". this job spins up the background vm agent, which first downloads your code from the azure repo and then executes the tasks in the yaml one by one (to build and publish it)
	2. Release pipeline (to release our built app into an azure web app for ex.)
		- azure web app deployment pipeline
		- add artifact (from where we are taking the app)
- Test Plans
- Artifacts

### ARM TEMPLATES in DevOps

Instead of going and deploying your resources one by one every time we need a new environment (based on a template); we can just go ahead and build this as an ARM template, which is **infrastructure as code**, upload it to Azure where the resource manager takes it and deploys the resources based on it. 

Whenever we need to create a new environment we submit the ARM template

**---->>*We can add it to the release pipeline, before the deploying step*<<----**

Based on JSON (also Bicep is a newer version)

## **Migration Cont.**

### Migration Patterns

If we are migrating our workloads from an on-premises environment onto azure
There are a few options of doing this migration:

1. ***Rehost*** - "lift-and-shift" migration
	- no code changes need to be made to our app
	- when we want to quickly transition
	- IaaS, use virtual machines as infrastructure (one vm for the app, one vm for the db)
2. ***Refactor*** 
	- May need minor code changes
	- PaaS, use services such as Azure Web App for our app and Azure SQL Database for db
3. ***Rearchitect*** your entire app
	- for example if your app is of monolithic structure, you might want to rebuild it using microservices and you might follow a devops strategy. you may containerize it and deploy on Azure Kubernetes and maybe use services such as Azure Cosmos DB and Cache for Redis

### Cloud Adoption Framework

Based on best practices when moving to the cloud.

Steps when moving:
1. Define the strategy: 
	- Why do you want to move to the cloud?
	- Can you reduce costs by moving to the cloud
	- Do you want to use cloud-native technologies?
2. Plan
	- What are we going to need to move to the cloud?
	- Which service do your apps needs to use when they move
	- Can we move your app as it is, for example: if its calling an external service; or consider the data part; or any legal reasons bc of which we may not be able to move the data to the cloud.
3. Keep cloud estate ready
4. Migrate your app and enhance by building new cloud-native components
5. Protect your resources
6. Manage the operations
7. Govern (Poilicy and compliance)

### Data transfer options to Azure Storage

***Questions to ask***:
- How much data do you want to transfer?
- Do you have enough bandwidth to transfer files over the internet?
- Is it a one time transfer of data?
	- *One time large transfer of data*
		For large amounts of data if you do not have enough bandwidth, the connection breaks and the data transfer fails. So instead we can use these utilities:
		1. Import Export/utility![[Pasted image 20240913154055.png]]
		2. Azure Data Box![[Pasted image 20240913154204.png]]
	- *Frequent transfer of changes (delta data)*
		If we just wanna transfer the change over time (or the delta data) we use these tools:
		1. Azure storage explorer
		2. AzCopy tool
		3. Azure Data Factory

***The AzCopy Tool***

A command line utility which is used to copy blobs or files TO of FROM your storage account. Create scripts that work with the azcopy tool
*Command example:* 
```
azcopy copy "C:\tmp4\*" "https://appstore443443.blob.core.windows.net/scripts?sv=2022-11-02&ss=b&srt=sco&sp=rwlac&se=2023-06-25T20:41:57Z&st=2023-06-25T12:41:57Z&spr=https&sig=KHqSPP4EOO0yAcD133MWx%2Bqah%2FJOjs5kNIQEIaJ6NE8%3D" --recursive
```


### Database Migration Service

Available in *Azure Data Studio* (which uses data factory internally)
- in Az Data Studio we search for the SQL Migration service and then create a connection with your sql server. Choose your migration target (Az SQL database, Az SQL Managed Instance or SQL Server on Az Virtual Machine)

![[Pasted image 20240913171928.png]]