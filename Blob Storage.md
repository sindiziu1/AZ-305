#data-storage 

***Access Tiering***

There is a cost for storing objects as well as accessing objects in storage accounts.
	1. *Hot Access tier* - high storage cost, low access cost. **Set at the storage account or blob level**
	2. *Cool Access tier* - lower storage costs but higher access costs compared with the High Access tier. Data needs to be stored for at least 30 days. **Set at the storage account or blob level**
	3. *Archive Access tier* - Lower storage costs but higher access costs when compared with the Cool Access tier. Good for long-term backups. Data needs to be stored for at least 180 days. Once blob is set on this tier, it ***cannot*** be accessed or downloaded anymore; unless we "rehydrate" it back to either Cool or Hot tier which may take several hours. **Set at the blob level**

