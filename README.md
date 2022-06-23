Isolation levels are used to define the degree to which one transaction must be isolated from resource or data modifications made by other concurrent transactions

## 1. READ UNCOMMITTED
- statements can read rows that have been modified by other transactions but not yet committed
- <ins>do not issue shared locks<ins> to prevent other transactions from modifying data read by the current transaction
- transactions are not blocked by exclusive locks at the time of data modification, thus <ins>allowing other transactions to read the modified data which is **NOT YET COMMITED**<ins>