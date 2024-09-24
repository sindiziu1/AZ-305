#monitoring

***Logs and Metrics***

[Azure Monitor Logs](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/data-platform-logs) lets you collect and organize data from resources that you monitor. You configure what data is gathered and how it's organized in the platform. Other features in Azure Monitor automatically store their data in Logs. 

[Azure Monitor Metrics](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/data-platform-metrics) captures numerical data from your monitored resources and stores the results in a time-organized database. Metrics are collected at intervals you specify. 

![[Pasted image 20240905134404.png]]

audit events - logs about database, need to configure the Diagnostic Settings

- Network security group - Flow Logs 
- Application Insights- where we can see the number of (get/post) requests in each page (ex. index, privacy) as well as the average durations of these operations in milliseconds ms

Check CPU utilization or any other indicator after a few weeks and notice whether we need an upgrade or downgrade on our cores or other resources.

***Azure Workbooks*** is a feature of Azure Monitor. The real power of Workbooks is the ability to combine data from disparate sources within a single report.

***Azure Insights*** 

| Insight                                                                                                                                      | Description                                                                                                                                                                                                                                                         |
| -------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Application Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)                                      | Monitor your live web application on any platform by using this extensible Application Performance Management (APM) service that's available in Azure Monitor.                                                                                                      |
| [Container insights](https://learn.microsoft.com/en-us/azure/azure-monitor/containers/container-insights-overview)                           | Check the performance of container workloads deployed to either Azure Container Instances or managed Kubernetes clusters hosted on Azure Kubernetes Service (AKS).                                                                                                  |
| [Networks insights](https://learn.microsoft.com/en-us/azure/azure-monitor/insights/network-insights-overview)                                | Obtain comprehensive information on the health and metrics for all your network resources. Use the advanced search capability to identify resource dependencies. Searching by your website name to locate resources that host your website.                         |
| [Resource group insights](https://learn.microsoft.com/en-us/azure/azure-monitor/insights/resource-group-insights)                            | Triage and diagnose any problems your individual resources encounter, while offering context as to the health and performance of the resource group as a whole.                                                                                                     |
| [Virtual machine insights](https://learn.microsoft.com/en-us/azure/azure-monitor/vm/vminsights-overview)                                     | Monitor your Azure Virtual Machines, Virtual Machine Scale Sets, and other virtual machines. Analyze the performance and health of your Windows and Linux Virtual Machines, and monitor their processes and dependencies on other resources and external processes. |
| [Azure Cache for Redis insights](https://learn.microsoft.com/en-us/azure/azure-monitor/insights/redis-cache-insights-overview)               | Review a unified, interactive report of overall performance, failures, capacity, and operational health.                                                                                                                                                            |
| [Azure Cosmos DB insights](https://learn.microsoft.com/en-us/azure/azure-monitor/insights/cosmosdb-insights-overview)                        | Get information on the overall performance, failures, capacity, and operational health of all your Azure Cosmos DB resources in a unified interactive experience.                                                                                                   |
| [Azure Key Vault insights](https://learn.microsoft.com/en-us/azure/azure-monitor/insights/key-vault-insights-overview)                       | Monitor your key vaults by using a unified report of your Key Vault requests, performance, failures, and latency.                                                                                                                                                   |
| [Azure Storage insights](https://learn.microsoft.com/en-us/azure/storage/common/storage-insights-overview?toc=/azure/azure-monitor/toc.json) | Do comprehensive monitoring of your Storage accounts via a unified report of your Storage performance, capacity, and availability.                                                                                                                                  |
