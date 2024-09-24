**Identity**
--
- ***Authentication*** - the process of logging in with a valid username and corresponding correct password.
- ***Authorization*** - the process of being provided access to different resources through Role Based Access Control (RBAC)
	 Provided at a resource level, resource group level (applying it to all resources under it) or at a subscription level (applies to all resource groups and all resources under them). Likewise the same works for Management Groups

Built in roles

Temporary roles (*eligible* instead of *active*)

Groups - assign roles to whole group of members, warning: they are non-changeable later

Access Reviews - select to review based on either groups or applications, and not based on roles (we can specify roles inside of the group). specify recurrence when creating it, auto apply results. After creation, we can complete the review in the MyAccess app

---
IMPORTANT    ***Managed Identity VS Identity Management VS Privileged Identity Management**
#vs
- **Azure AD Managed Identity**: Provides Azure resources (e.g., VMs, app services) with an automatically managed identity in Azure AD. This identity allows the resource to authenticate securely to other Azure services without storing credentials in code, enhancing security by eliminating the need to manage secrets.
    
- **Azure AD Identity Management**: Encompasses the management of all user identities, access controls, and authentication within an organization’s Azure AD. It includes user lifecycle management (creation, updating, deletion), enforcing security policies (like MFA and conditional access), and providing single sign-on (SSO) and identity governance for all users.
    
- **Azure AD Privileged Identity Management (PIM)**: Focuses on managing, monitoring, and controlling elevated (privileged) roles within Azure AD and Azure. PIM provides just-in-time (JIT) access to critical resources, requires MFA for role activation, performs access reviews, and alerts on suspicious activities, reducing the risk of over-permissioned access.
---
### Identity Protection Service

Detects these different risks 
- Leaked user credential
- Anonymous IP Address
- Atypical different geographic locations
- Whether users device could be infected with malware
- Whether someone is trying out different multiple passwords
- Atypical behaviors
We can add ***policies*** in case of user or login risks, such as requesting password change or MFA

### Policy Service

There's multiple Built-In policies here on different risk factors (ex. location), which we can apply onto different levels of the hierarchy: the whole tenant group, specific subscription levels or resource groups. 

-----------

***Resource Locks*** - created at resource, resource group or subscription level. if applied to group, all resources also get it. Example: ReadOnlyLock. We can delete them afterwards

------
***Azure Blueprints -*** Used to orchestrate the deployment of artifacts to Azure Platform. Having a set of predefined rules in place to apply to any new subscription added to the platform: 
- a standard set of *resource groups*, 
- a standard set of *resources* that need to be deployed via *ARM Templates* (Azure Resource Management), 
- a standard set of *Azure Policies* 
- and *RBAC*; 
  -> all of these can be grouped in a blueprint which can all be assigned to any new subscription which is under the management group 

Either saved to a management group or a subscription (need to have contributor access)
Ensure to add the Contributor role (in Management group) to yourself in order to create Blueprint

We can also apply resource locks in a blueprint (ex. a do-not-delete lock). This stops even the OWNER to not be able to delete the resource for as long as the blueprint is assigned. The lock does not appear at all in the locks section of the administrator bc it is being managed by the blueprint

---
*Application Object* - if we need to grant an app access into an Az Storage Account using Role-based access control (which is the more secure way)
- We create an app object and copy its tenant ID into our code in the app. then we create a secret and copy its value there as well (client ID, client secret value). MUST COPY IMMEDIATELY as after we remove the page we cant access it anymore; we have to delete and recreate it.

---
*Managed Identity*
If we have an existing virtual  machine with our app in it, we simply Enable the Managed Identity for it. An azure entra ID gets automatically created which can then give the app access to the storage account. Use TokenCredential on the app code itself

By default, each resource has a system-assigned managed identity, which gets automatically deleted when we delete the resource. 
On the other hand we have the user-assigned managed identities, which after creation we can assign to *multiple resources*, in the case where we need to give the same permissions to stuff for all these different resources and not lose time into separately managing the roles and access for each.

---
# Security

Azure Key Vault Service
--
Used for managing *Secrets*, *Encryption Keys and Certificates*. 
To store a database password that is used by an app, we make use of the secrets stored in the Azure Key Vault; to store an SSL certificate for our app; to store the encryption key if we would like to encrypt our vm disk.

To use a secret in our app, first we add the vault URL into the code


*Azure Entra ID Application Proxy
--
Scenario: ***The company has a hybrid infrastructure of both Azure and on-premises.*** They have users created in both and apps published in both. What we need is for the users in Azure to be able to access the application hosted on-premises. One way to do this would be to publish the app in Azure, exposing it to the internet. But this adds a security risk. 
->Instead we use the *Azure Application Proxy* feature.
![[Pasted image 20240906105023.png]]

As we can see above is the infrastructure for our on-premises setup. The web app is hosted on our local web server. Local users are managed through the Microsoft Active Directory. We can add another server in which we install a light weight tool known as a Proxy Connector app. What it does is it works with the *Azure Application Proxy* service from the Azure Entra, getting the user tokens from Azure and then using them to access the on-premises app

### ---> OTHER WAY AROUND 

How can users defined in the on-premises Microsoft Active Directory, access the resources defined in Azure Active Directory. We can do this by:
- Using the *Active Directory Federation Services*, by using the user identities that have already authenticated themselves via the on-premises AD. 
- Using the *Azure AD Connect tool*. It synchronizes the users directly, from the on-premises to Azure, so we only have to create them once




---
Multi Factor Authentication
--
---
*Enterprise Apps*
--
*Single Sign-On*
Integrate Azure Entra as an identity provider to configure single sign-on in all of the external enterprise apps, listed in the portal.
1. Azure needs to support single sign-on with that specific external app (could be a self made app, not necessarily official enterprise)
2. This app needs to support single sign-on with Azure (vice-versa as above)
3. Configure sign-on in both areas

### *Conditional Access Policies**
Add MFA for the single sign-on process 