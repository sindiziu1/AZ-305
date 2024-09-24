#monitoring #threat-protection #security
A cloud service that provides a solution for SEIM (Security information Event Management) and SOAR (Security Orchestration Automated Response).

***---If you want to gather information when it comes onto the different security events that are happening around you and if you want to have an automated response in place in case of any security threats, we can make use of the Microsoft Sentinel Service---***

- Collection of data - using a variety of connectors; we have connectors for a variety of Microsoft products and other third-party products as well. We can then use built-in workbooks to get more insights on the collected data
- Detect undetected threats
- hunt suspicious activities at scale
- helps to respond to incident rapidly

We can use the Microsoft Sentinel as an additional service on top of a Log Analytics workspace, so a Log Analytics workspace must be in place beforehand.

Enable a Data Connector, where we can create a Data Collection Rule applied in it, collects events from a specific source in your selected resources.

Microsoft Sentinel Analytics, we have to define an analytics query rule, with a specific query (using event ID), according to our needs. An incident gets raised  we can map to specific tactics and techniques on how to handle it based on the event type.
Alert threshold, we select it ourselves with the number of query results we would accept until an alert is raised, an incident is raised for each alert.
Each incident can be then assigned to a security engineer which can continue with resolving the incident.