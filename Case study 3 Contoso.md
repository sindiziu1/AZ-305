**Overview** - Contoso, Ltd. is a research company that has a main office in Montreal.

---

**Existing Environment**

**Technical Environment** - The on-premises network contains a single Active Directory domain named contoso.com. Contoso has a single Azure subscription.

**Business Partnerships** - Contoso has a business partnership with Fabrikam, Inc. Fabrikam users access some Contoso applications over the internet by using Azure Active Directory (Azure AD) guest accounts.

---

**Requirements**

**Planned Changes** - Contoso plans to deploy two applications named App1 and App2 to Azure.

---

**App1** - App1 will be a Python web app hosted in Azure App Service that requires a Linux runtime. Users from Contoso and Fabrikam will access App1. App1 will access several services that require third-party credentials and access strings. The credentials and access strings are stored in Azure Key Vault.

- App1 will have six instances: three in the East US Azure region and three in the West Europe Azure region.
    
- App1 has the following data requirements:
    
    - Each instance will write data to a data store in the same availability zone as the instance.
    - Data written by any App1 instance must be visible to all App1 instances.
    - App1 will only be accessible from the internet.
    
- App1 has the following connection requirements:
    
    - Connections to App1 must pass through a web application firewall (WAF).
    - Connections to App1 must be active-active load balanced between instances.
    - All connections to App1 from North America must be directed to the East US region.
    - All other connections must be directed to the West Europe region.
- Every hour, a maintenance task will run by invoking a PowerShell script that copies files from all the App1 instances. The PowerShell script will run from a central location.
    

---

**App2** - App2 will be a .NET app hosted in App Service that requires a Windows runtime.

- App2 has the following file storage requirements:
    
    - Save files to an Azure Storage account.
    - Replicate files to an on-premises location.
    - Ensure that on-premises clients can read the files over the LAN by using the SMB protocol.
- You need to monitor App2 to analyze how long it takes to perform different transactions within the application. The solution must not require changes to the application code.
    

---

**Application Development Requirements** - Application developers will constantly develop new versions of App1 and App2. The development process must meet the following requirements:

- A staging instance of a new application version must be deployed to the application host before the new version is used in production.
- After testing the new version, the staging version of the application will replace the production version.
- The switch to the new application version from staging to production must occur without any downtime of the application.

---

**Identity Requirements** - Contoso identifies the following requirements for managing Fabrikam access to resources:

- Every month, an account manager at Fabrikam must review which Fabrikam users have access permissions to App1.
- Accounts that no longer need permissions must be removed as guests.
- The solution must minimize development effort.

---

**Security Requirement** - All secrets used by Azure services must be stored in Azure Key Vault. Services that require credentials must have the credentials tied to the service instance. The credentials must **NOT** be shared between services.

---
**Question**s:

1. You need to recommend an App Service architecture that meets the requirements for App1. The solution must minimize costs. What should you recommend?



2.  You need to recommend a solution that meets the data requirements for App1. What should you recommend deploying to each availability zone that contains an instance of App1?



3. You need to recommend a solution for the App1 maintenance task. The solution must minimize costs. What should you include in the recommendation? 



4. You need to recommend a solution that meets the application development requirements. What should you include in the recommendation?



5. What should you recommend to meet the monitoring requirements for App2?