**HealthEngine, Inc.** is an engineering company that has offices throughout Europe. The company has a main office in London and three branch offices in Amsterdam, Berlin, and Rome.

---

### Existing Environment

**Active Directory Environment**

- The network contains two Active Directory forests named:
    - **corp.HealthEngine.com** (Production forest for internal user and computer authentication)
    - **rd.HealthEngine.com** (Used by the research and development (R&D) department only)
- There are no trust relationships between the forests.
- **Corp.HealthEngine.com** is used for internal user and computer authentication.
- **Rd.HealthEngine.com** is restricted to using on-premises resources only.

**Network Infrastructure**

- Each office contains at least one domain controller from the **corp.HealthEngine.com** domain.
- The main office contains all the domain controllers for the **rd.HealthEngine.com** forest.
- All the offices have a high-speed connection to the Internet.
- An existing application named **WebApp1** is hosted in the data center of the London office.
    - **WebApp1** is used by customers to place and track orders.
    - **WebApp1** has:
        - A web tier using Microsoft Internet Information Services (IIS)
        - A database tier running Microsoft SQL Server 2016
    - Both tiers are deployed to virtual machines running on Hyper-V.
- The IT department currently uses a separate Hyper-V environment to test updates to **WebApp1**.
- HealthEngine purchases all Microsoft licenses through a Microsoft Enterprise Agreement that includes Software Assurance.

---

### Existing Environment - Problem Statements

- The use of **WebApp1** is unpredictable. At peak times, users often report delays. At other times, many resources for **WebApp1** are underutilized.

---

### Requirements - Planned Changes

- HealthEngine plans to move most of its production workloads to Azure during the next few years.
- As one of its first projects, the company plans to:
    - Establish a hybrid identity model
    - Facilitate an upcoming Microsoft Office 365 deployment
- All R&D operations will remain on-premises.
- HealthEngine plans to migrate:
    - The production and test instances of **WebApp1** to Azure
    - To use the S1 plan.

---

### Requirements - Technical Requirements

HealthEngine identifies the following technical requirements:

- Web site content must be easily updated from a single point.
- User input must be minimized when provisioning new web app instances.
- Whenever possible, existing on-premises licenses must be used to reduce cost.
- Users must always authenticate by using their **corp.HealthEngine.com** UPN identity.
- Any new deployments to Azure must be redundant in case an Azure region fails.
- Whenever possible, solutions must be deployed to Azure by using platform as a service (PaaS).
- An email distribution group named **IT Support** must be notified of any issues relating to the directory synchronization services.
- Directory synchronization between Azure Active Directory (Azure AD) and **corp.HealthEngine.com** must not be affected by a link failure between Azure and the on-premises network.

---

### Requirements - Database Requirements

HealthEngine identifies the following database requirements:

- Database metrics for the production instance of **WebApp1** must be available for analysis so that database administrators can optimize the performance settings.
- To avoid disrupting customer access, database downtime must be minimized when databases are migrated.
- Database backups must be retained for a minimum of seven years to meet compliance requirements.

---

### Requirements - Security Requirements

HealthEngine identifies the following security requirements:

- Company information including policies, templates, and data must be inaccessible to anyone outside the company.
- Users on the on-premises network must be able to authenticate to **corp.HealthEngine.com** if an Internet link fails.
- Administrators must be able to authenticate to the Azure portal by using their **corp.HealthEngine.com** credentials.
- All administrative access to the Azure portal must be secured by using multi-factor authentication.
- The testing of **WebApp1** updates must not be visible to anyone outside the company.

---
## Question 

1. You are evaluating the components of the migration to Azure that require you to provision an Azure Storage account. For each of the following statements, select Yes if the statement is true. Otherwise, select No. 
	1. You must provision an Azure Storage account for the SQL Server database migration. 
	2. You must provision an Azure Storage account for the Web site content storage. 
	3. You must provision an Azure Storage account for the Database metric monitoring. 
		
		**ANSWER: No,No,Yes**

3. What should you include in the identity management strategy to support the planned changes?

4.  You need to recommend a solution to meet the database retention requirement. What should you recommend?