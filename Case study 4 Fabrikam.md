**Fabrikam, Inc.** is an engineering company that has offices throughout Europe. The company has a main office in London and three branch offices in Amsterdam, Berlin, and Rome.

---

**Existing Environment:**

**Active Directory Environment**  
The network contains two Active Directory forests named **corp.fabrikam.com** and **rd.fabrikam.com**. There are no trust relationships between the forests.

- **Corp.fabrikam.com** is a production forest that contains identities used for internal user and computer authentication.
- **Rd.fabrikam.com** is used by the research and development (R&D) department only. The R&D department is restricted to using on-premises resources only.

---

**Network Infrastructure**  
Each office contains at least one domain controller from the **corp.fabrikam.com** domain. The main office contains all the domain controllers for the **rd.fabrikam.com** forest. All the offices have a high-speed connection to the internet.

An existing application named **WebApp1** is hosted in the data center of the London office. WebApp1 is used by customers to place and track orders. WebApp1 has a web tier that uses Microsoft Internet Information Services (IIS) and a database tier that runs Microsoft SQL Server 2016. The web tier and the database tier are deployed to virtual machines that run on Hyper-V. The IT department currently uses a separate Hyper-V environment to test updates to WebApp1.

Fabrikam purchases all Microsoft licenses through a **Microsoft Enterprise Agreement** that includes Software Assurance.

---

**Existing Environment: Problem Statements**  
The use of WebApp1 is unpredictable. At peak times, users often report delays. At other times, many resources for WebApp1 are underutilized.

---

**Requirements: Planned Changes**  
Fabrikam plans to move most of its production workloads to **Azure** during the next few years, including virtual machines that rely on Active Directory for authentication. As one of its first projects, the company plans to establish a **hybrid identity model**, facilitating an upcoming **Microsoft 365 deployment**. All R&D operations will remain on-premises.

Fabrikam plans to migrate the production and test instances of **WebApp1** to Azure.

---

**Requirements: Technical Requirements**  
Fabrikam identifies the following technical requirements:

- Website content must be easily updated from a single point.
- User input must be minimized when provisioning new web app instances.
- Whenever possible, existing on-premises licenses must be used to reduce cost.
- Users must always authenticate by using their **corp.fabrikam.com UPN** identity.
- Any new deployments to Azure must be redundant in case an Azure region fails.
- Whenever possible, solutions must be deployed to Azure by using the **Standard pricing tier** of Azure App Service.
- An email distribution group named **IT Support** must be notified of any issues relating to the directory synchronization services.
- In the event that a link fails between Azure and the on-premises network, ensure that the virtual machines hosted in Azure can authenticate to Active Directory.
- Directory synchronization between **Azure Active Directory (Azure AD)** and **corp.fabrikam.com** must not be affected by a link failure between Azure and the on-premises network.

---

**Requirements: Database Requirements**  
Fabrikam identifies the following database requirements:

- Database metrics for the production instance of WebApp1 must be available for analysis so that database administrators can optimize the performance settings.
- To avoid disrupting customer access, database downtime must be minimized when databases are migrated.
- Database backups must be retained for a minimum of seven years to meet compliance requirements.

---

**Requirements: Security Requirements**  
Fabrikam identifies the following security requirements:

- Company information including policies, templates, and data must be inaccessible to anyone outside the company.
- Users on the on-premises network must be able to authenticate to **corp.fabrikam.com** if an internet link fails.
- Administrators must be able to authenticate to the Azure portal by using their **corp.fabrikam.com** credentials.
- All administrative access to the Azure portal must be secured by using **multi-factor authentication (MFA)**.
- The testing of WebApp1 updates must not be visible to anyone outside the company.

---

**Questions** 
1. You need to recommend a notification solution for the IT Support distribution group. What should you include in the recommendation?



2. What should you include in the identity management strategy to support the planned changes?



3. You need to recommend a data storage strategy for WebApp1.What should you include in the recommendation?



4. You need to recommend a solution to meet the database retention requirements. What should you recommend?



5. What should you include in the identity management strategy to support the planned changes?