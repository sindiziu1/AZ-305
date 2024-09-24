What happens if the Virtual Machine hosting the application goes down?
What happens if the VM hosting the SQL database goes down?
Do you have backup strategies in place?
How much revenue is lost when an outage occurs?

aim is for **HIGH AVAILABILITY**

**RPO - Recovery Point Objective**![[Pasted image 20240911134540.png]]

**RTO - Recovery Time Objective**
![[Pasted image 20240911134839.png]]


Azure SQL Database
--
### ***Backup and restore***

For every SQK db, the engine maintains a transaction log which records all "transactions" done in the db. It can greatly help with backup in case of an outage. For example, backup once a day in the morning and then incrementally backup from the transaction file, bc backing up the whole db continuously is expensive and not so safe
- Full backup, includes the transaction logs --> ***once a week***
- Differential backup --> ***every 12 to 24 hours***
- Transaction log backup --> ***every 10 minutes***

![[Pasted image 20240911141231.png]]

By default, backups are retained for 7 days, but you can extend it up to 35 days.

Disks are part of the backup.

Back up process and the restore process take time to complete.

### ***Active Geo-Replication***
Replicate your db in db server in another region. Continuously synched with the primary. Geo-replica is a read-only replica db.
This we can use for:
- reports generation, in order to not stress our primary db
- for a recovery in case of a fail on the primary (failover from primary to secondary,  meaning secondary takes the primary role)
We can create multiple replicas to accommodate both these use cases separately for example.

***Auto-Failover Groups***
A feature of the Azure SQL database, built on top of Geo-replication. Failover is either initiated by you or you can have a policy in place that can carry out the failover process. You should change the database name but you don't need to change the connection string in your application code which writes data on the db

***In Premium Tier of SQL db:***
- the secondary replicas are created automatically
- Data changes are constantly pushed from the primary to the secondary replica
- Option of zone-redundancy, data gets copied to multiple availability zones
- No data loss from the primary to the secondary replica, as the transactions are only committed once the data has been pushed to the secondary replica.

Availability Sets
--
ALL WITHIN ONE DATA CENTER!!

***Update Domains:*** A group of machines and their underlying physical hardware that can be rebooted at the same time. In case of updates on the servers, done one group at a time
***Fault Domains:*** A group of machines that share a common power source and network switch. In case of a power outage or other issue, only one fault domain group affected

Each machine you create is assigned a particular Update and Fault Domain.

Only makes sense if we have multiple machines deployed as part of our app infrastructure.

Will the availability set synchronize the data across the machines? - NO, this is our responsibility. Availability only assures distribution through different networks etc.

Availability Zones
--
IN UNIQUE PHYSICAL LOCATIONS, equipped with independent power, cooling and networking.

Here we have a data transfer charge.

No data synchronization included.

Usually 3 av zones per region

Azure Backup
--
***For VMs:***
Provides access to the data on the VM if sth happens to the originl VM.
Backup data gets written to a Azure Site Recovery Services Vault, which *needs to be on the same geo region as our VM*s, for example both East US.

Enable backup for the machine via the use of a Recovery Services Vault.

Trigger recovery Snapshot of the db, create new recovery VM

2 disks - one for the data, one for the logs

On your sql db resource -> go to Backup -> set your backup Vault there after creating the vault

To *restore a VM*, we need a storage account to use as a staging environment. Create restore job

### **Key Differences between RPO and RTO:**

|**Aspect**|**Recovery Point Objective (RPO)**|**Recovery Time Objective (RTO)**|
|---|---|---|
|**Focus**|Data Loss|Downtime|
|**Definition**|Maximum acceptable amount of data loss|Maximum acceptable time to restore services|
|**Measurement**|Time between the last backup and the disaster|Time taken to recover from a disaster|
|**Purpose**|Minimize data loss in case of a disaster|Minimize downtime and its impact|
|**Example**|Backup every 4 hours (RPO = 4 hours)|Recovery within 2 hours (RTO = 2 hours)|

***How RPO and RTO Work Together:***
- **RPO** focuses on data availability and determines how frequently backups should occur.
- **RTO** focuses on service availability and determines how quickly systems must be brought back online.

Azure Site Recovery
--
### Disaster recovery

- New virtual network and resource group created

Continuous replication of data from src onto dest. --> **RPO is MUCH QUICKER**

Test Failover option

Blob data protection
--
***Blob soft delete -*** Helps to protect the individual blob from accidental deletion
	Deleted data is kept on the system for a defined duration of time. You can specify a retention period between 1 and 365 days. Same works for a container as well, to protect the entire

***Versioning*** - Used to maintain the previous version of a blob
	When a blob is modified, a new version ID is created for the blob.
	Blob snapshots -> a read only version of a blob, taken at a particular point in time


