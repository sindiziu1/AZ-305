#data-storage #storage 
Microsoft-Managed keys
--
by default

Customer-Managed Keys
--



Transparent Data Encryption
--
Automatic, managed by the platform itself. although optional to use your own management
Data Masking
--
This does not affect admin users; if not an admin, the masked column will appear as masked on the db when we select
1. *Exposure* - Here you can limit the exposure of data
2. *Rule* - you can create rules to mask the data
3. *Credit Card masking rule* - This is used to mask the column that contains credit card details, only the last four digits are exposed.
4. *Email* - only first letter of the email address is exposed and domain name is replaced with xxx.com
5. *Custom text* - you decide which characters to expose for a field
6. *Random number* - Generate a random number for the field

Always Encrypted
--
Can be used to encrypt data at rest and in motion (during migration).

***Deterministic encryption*** - the same encrypted value is generated for any given plain text value. This is ***less secure***, but it allows for point lookups, equality joins, grouping and indexing on encrypted columns.
***Randomized encryption*** - This is the ***most secure*** encryption method. But it prevents the searching grouping indexing and joining on encrypted columns.

We can save our encryption key in Azure Key Vault