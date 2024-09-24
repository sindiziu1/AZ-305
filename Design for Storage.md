***Azure SQL Database Service is a PaaS*** , a mirror of Microsoft SQL Server on the web

***Azure Cosmos DB*** - where we can store non-relational data (no SQL)

***Azure Synapse*** - Used to host data in a data warehouse. This data is transferred from a db into Synapse using ***Azure Data Factory***. Then we can process and analyze the data using ***Azure Databricks***
	 ***DATA****  --->  ***Azure Data Factory***  --->  ***Azure Synapse***   ---> ***Azure Databricks***
		data* might come from different sources: Log Analytics workspace, [[Azure Data Lakes]] storage account or streaming data from other sources

***Azure Storage Explorer***
A downloadable program where you can manage storage accounts blobs using *Account Keys* 

***[[Azure Data Explorer]] (ADX)***
Analytics service optimized for big data, large-scale and near real-time analytics. 

**ACCESS**
- Access Key
- Shared Access Signature in storage acc
- Azure Active Directory using Role Based Access Control RBAC; in Access Control role assignment of storage resource

---
### Premium Storage Accounts

Used when we need high performance when it comes to storage and access to data
Data in the background is stored on SSDs, optimized for low latency. File transfer is also much faster. 
*Workloads:* Streaming, Machine Learning
Higher storage costs but lower transaction costs
***Here we cant set the access tiers.***

### Data Redundancy

When we create a standard storage account, we have these data redundancy options to choose from:
- Locally-redundant storage (LRS)
By default azure gives LRS, making three copies of your data inside one data center, to deal with server rack or drive failure.

- Zone-redundant (ZRS)
Three copies of data saved across three availability zones, helping to protect against data center failures (power, cooling, networking)

- Geo-redundant (GRS)
Data replicated to another region completely, *saving three copies in each region, both the main region and its PAIR region*.

- Geo-zone-redundant (GZRS)
Mix of geo&zone. In the primary region, there are three copies made across different availability zones, while on the pair region all three copies are saved in one data center.

Relational data
--
### ***Microsoft SQL Server*** on Azure
1. *IaaS 
2. *PaaS
3. Azure SQL Managed Instance - An ideal option for migrating existing SQL Server workloads onto Azure. Has a near 100% compatibility with the latest SQL Server db engine and you also get native Virtual Network Integration

1. ***Iaas*** - Using a virtual machine* to install SQL Server. You have full control over infrastructure:
	When we create VM we can select image as a SQL Server, so its just like a regular Virtual Machine but there's a SQL Server on the machine. We can connect to it from Microsoft SQL Server Management Studio, using server address (public IP address) and password that we should have filled in when we created VM

2. ***PaaS - Using Azure SQL database Service*. Here underlying infrastructure is managed by Azure***
	Create new SQL database in Azure
	
	*[Transparent data encryption](Encryption) (automatic, managed by the platform itself. although optional to use your own management)*

	***[Dynamic Data Masking](Encryption)*** - does not affect admin users, otherwise the specified column will appear as masked on the db when we select
	1. *Exposure* - Here you can limit the exposure of data
	2. *Rule* - you can create rules to mask the data
	3. *Credit Card masking rule* - This is used to mask the column that contains credit card details, only the last four digits are exposed.
	4. *Email* - only first letter of the email address is exposed and domain name is replaced with xxx.com
	5. *Custom text* - you decide which characters to expose for a field
	6. *Random number* - Generate a random number for the field

	[Always Encrypted](Encryption)
	Can be used to encrypt data at rest and in motion (during migration).
	
	***Deterministic encryption*** -
	***Randomized encryption*** - 

We can save our encryption key in Azure Key Vault


Non-Relational Data
--
### Azure Cosmos DB

When we don't have structured data.
Fully managed NoSQL database system. 
Here the infrastructure is managed for you. 
Fast access to data.
Create multiple read and write replica's of your data to multiple locations

Multiple API's to choose from when creating a new db like: MongoDB, NoSQL, Cassandra, Gremlin, Table. Helpful especially when migrating an existing db into Azure, use Cosmos DB with the specific API configuration from where its coming from, for ex. if they are already using a Cassandra based DB, we can use the Cassandra API.

In NoSQL - we add data in the form of JSON based objects. Many JSON objects stored inside of a container in our db.

***Partition Key:***
Group data into various partitions inside the container for faster accessibility and searching when we have a lot of data, group by category for example (category would become our partition key), when we have a category field in our JSON objects. When we have little data we can just set the ID as a partition key.

*Replicate Data Globally:*
We can replicate our data in different regions, for Reading as well as Writing. We also have ***different levels of consistency***, from writing in one region, to the data being updated in the other read-only regions.



BIG DATA
--
Working with large data sets, data arriving in large volumes and at a fast rate.

### [[Azure Data Lake Storage Gen2]]

--> A service built on top of Azure Blob Storage which gives you the ability to host an enterprise data lake on Azure. Helps organize the objects/files into a hierarchy of directories for efficient data access.

A data lake is used to store large amounts of data in its native, raw format. It is optimized for storing terabytes and petabytes of data coming from a variety of data sources and in various formats: structured, semi-structured and unstructured data.

**how to create:** when creating a storage account, *enable hierarchical namespace* in advanced part. so it acts mostly as a normal storage account. although it has an extra feature, add a directory inside of container where you can upload blob files in different formats.

Data Warehouse
--
Centralized repository of data taken from different data sources. Normally used by businesses for analyzing data. For example, to compute the purchases done per month per region; most popular courses for the month per region; times of the day during which most purchases are made etc.

We copy data from the database into the warehouse (powered by the same engine) where we can perform the analysis, without stressing our main db. It needs a lot of processor power to analyze.

Built for dealing with large amounts of data.

Business users can also use reporting tools to visualize the data.

---------***[[Azure Synapse Service]]***----------
Create a ***Synapse workspace*** on top of an existing Data Lake Storage Gen2 account

There are two compute options:
*Serverless SQL Pool* & *SQL Pool*
SQL Pool is more expensive, but the data is already structured and ready to be queried and visualized by business users. It also has dedicated compute power.

| *Serverless SQL Pool*                            | *SQL Pool*                                         |
| ------------------------------------------------ | -------------------------------------------------- |
| Used to perform quick adhoc <br>analysis of data | Used to build your data<br>warehouse               |
| Use T-SQL to work with <br>your data             | if you need to persist your data                   |
| Charged based on how<br>much you use             | Compute nodes will be used to <br>process the data |
|                                                  | Charged based on a metric known as DWU             |
Then we can access it just like a regular SQL database in SSMS:![[Pasted image 20240909163549.png]]
So the core difference is the purpose; while on a normal database we mainly just collect and store the data, on a data warehouse we do all of the analysis *so we get much more dedicated computing power for it.*

Azure Synapse Link for Cosmos DB
--
**Cosmos** - noSQL, non-structured operational data

Enable Azure Synapse link on our Azure Cosmos DB account and then link it on the synapse workspace. So in our workspace we can have both our Data Lake account and our linked Cosmos DB at the same time

We can then use Apache Sparks "dataframe" to load our data into a dataframe in Synapse and analyze it there.


Databricks
--
***Purpose:*** Address and solve matters that come up when you use Azure Data Lakes.  

Instead of transferring our data from a Data Lake into a Data Warehouse, to continue with operations, we can directly apply the warehouse skills on top of the Data Lake, creating a Data LakeHouse.

--> Allowance for data to be in different formats ; transactional support;  SCHEMA MANAGEMENT on a data Lake ; Atomicity, Consistency, Isolation, Durability.

Databricks Lakehouse architecture 
![[Pasted image 20240910173501.png]]


Azure Data Factory
--
 ***A cloud-based ETL tool*** (meaning: Extract, Transform, Load—is a data integration process that combines, cleans and organizes data from multiple sources into a single, consistent data set for storage in a data warehouse, data lake or other target system)\

Here you create a pipeline for different purposes.

The pipeline executes in the background on underlaying compute infrastructure that's managed FOR you

Use PolyBase instead of bulk insert, when transferring large amounts of data from lets say an Azure DataLake Gen 2 storage account.

When files are not located in a data Lake, but rather on a VM or an on-premises server, we make use of a self-hosted runtime to transfer the data