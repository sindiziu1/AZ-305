#service #storage #big-data 
Create a Synapse **workspace** on top of an existing Data Lake Storage Gen2 account

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

***Synapse Studio***
has an integrated environment where we can access our db, just like we would in SSMS. it also has a pipeline feature
Pipeline
--
Synapse provides a wizard which walks us through multiple types of integration pipelines creation such as: Link connection, Copy Data tool or you can import another template. 
This provides us with a pipeline the data goes through in the background, in the cases where we need to do some modification to it before we actually analyze

