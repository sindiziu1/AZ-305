
**Existing Environment:**

- **Technical Environment:** The on-premises network contains a single Active Directory domain named HealthEngine.com. HealthEngine has a single Azure subscription.
    
- **Business Partnerships:** HealthEngine has a business partnership with ClinicLabs, Inc. ClinicLabs users access some HealthEngine applications over the internet by using Azure Active Directory (Azure AD) guest accounts.
    

**Requirements:**

- **Planned Changes:** HealthEngine plans to deploy two applications named App1 and App2 to Azure.
    
    - **App1:**
        
        - App1 will be a Python web app hosted in Azure App Service that requires a Linux runtime.
        - Users from HealthEngine and ClinicLabs will access App1.
        - App1 will access several services that require third-party credentials and access strings. The credentials and access strings are stored in Azure Key Vault.
        - App1 will have six instances: three in the East US Azure region and three in the West Europe Azure region.
        - **Data Requirements:**
            - Each instance will write data to a data store in the same availability zone as the instance.
            - Data written by any App1 instance must be visible to all App1 instances.
            - App1 will only be accessible from the internet.
        - **Connection Requirements:**
            - Connections to App1 must pass through a web application firewall (WAF).
            - Connections to App1 must be active-active load balanced between instances.
            - All connections to App1 from North America must be directed to the East US region.
            - All other connections must be directed to the West Europe region.
            - Every hour, you will run a maintenance task by invoking a PowerShell script that copies files from all the App1 instances. The PowerShell script will run from a central location.
    - **App2:**
        
        - App2 will be a .NET app hosted in App Service that requires a Windows runtime.
        - **File Storage Requirements:**
            - Save files to an Azure Storage account.
            - Replicate files to an on-premises location.
            - Ensure that on-premises clients can read the files over the LAN by using the SMB protocol.
        - You need to monitor App2 to analyze how long it takes to perform different transactions within the application. The solution must not require changes to the application code.

**Application Development Requirements:**

- Application developers will constantly develop new versions of App1 and App2. The development process must meet the following requirements:
    - A staging instance of a new application version must be deployed to the application host before the new version is used in production.
    - After testing the new version, the staging version of the application will replace the production version.
    - The switch to the new application version from staging to production must occur without any downtime of the application.

**Identity Requirements:**

- HealthEngine identifies the following requirements for managing ClinicLabs access to resources:
    - Every month, an account manager at HealthEngine must review which ClinicLabs users have access permissions to App1. Accounts that no longer need permissions must be removed as guests.
    - The solution must minimize development effort.

**Security Requirement:**

- All secrets used by Azure services must be stored in Azure Key Vault.
- Services that require credentials must have the credentials tied to the service instance.
- The credentials must NOT be shared between services.

**Question:** 

1. What should you implement to meet the identity requirements?
	**ANSWER: 
		Service: Azure AD Identity Governance
		Feature: Access reviews**

2.  You need to recommend a solution that meets the data requirements for App1. What should you recommend deploying to each availability zone that contains an instance of App1?
	**ANSWER: an Azure Cosmos DB that uses multi-region writes**

3. You need to recommend a solution to ensure that App1 can access the third-party credentials and access strings. The solution must meet the security requirements. What should you include in the recommendation?
	**ANSWER: 
		Authorize App1 to retrieve Key Vault secrets by using: an access policy
		Authenticate App1 by using: A system-assigned managed identity**


4. You need to recommend a solution that meets the file storage requirements for App2. What should you deploy to the Azure subscription and the on-premises network?
		**ANSWER: 
			**