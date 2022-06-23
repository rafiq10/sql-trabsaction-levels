# TSQL ISOLATION LEVEL

*Inspired by:*
[Source: isolation-levels-in-sql-server](https://www.sqlservercentral.com/articles/isolation-levels-in-sql-server "isolation-levels-in-sql-server")
## INTRO
Isolation levels are used to define the degree to which one transaction must be isolated from resource or data modifications made by other concurrent transactions

- Only one of the isolation level options can be set at a time, and it remains set for that connection until it is explicitly changed
- A lower isolation level increases the ability of many users to access data at the same time, but increases the number of concurrency effects, such as dirty reads or lost updates etc.
- a higher isolation level reduces the types of concurrency effects that users might encounter, but requires more system resources and increases the chances that one transaction will block another

## 1. READ UNCOMMITTED
- statements can read rows that have been modified by other transactions but not yet committed
- <ins>do not issue shared locks</ins> to prevent other transactions from modifying data read by the current transaction
- transactions are not blocked by exclusive locks at the time of data modification, thus <ins>allowing other transactions to read the modified data which is **NOT YET COMMITED**</ins>
- Reading uncommitted modifications is known as a Dirty Read.

## 2. READ COMMITTED
- transactions issue exclusive locks at the time of data modification, thus not allowing other transactions to read the modified data that is not yet committed
- prevents the Dirty Read
- data can be changed by other transactions between individual statements within the current transaction
    => Non-repeatable Read (Phantom Row)

## 3. REPEATABLE READ
- statements cannot read data that has been modified but not yet committed by other transactions
- no other transaction can modify data that has been read by the current transaction until the current transaction completes
- shared locks are placed on all data read by each statement in the transaction and are held until the transaction completes
- this prevents other transactions from modifying any rows that have been read by the current transaction
- prevents the Non-Repeatable Read issue
- other transactions can insert new rows that match the search conditions of statements issued by the current transaction
- if the current transaction then retries the statement it will retrieve the new rows, which results in **phantom reads**

## 4. SERIALIZABLE
- statements cannot read data that has been modified but not yet committed by other transactions
- no other transactions can modify data that has been read by the current transaction until the current transaction completes
- other transactions cannot insert new rows with key values that would fall in the range of keys read by any statements in the current transaction until the current transaction completes
- phantom Rows issue is resolved

## 5. SNAPSHOT ISOLATION
- data read within a transaction will never reflect changes made by other simultaneous transactions
- data read by any statement in a transaction will be the transactional consistent version of the data that existed at the start of the transaction
- data modifications made by other transactions after the start of the current transaction are not visible to statements executing in the current transaction
- snapshot transactions do not request locks when reading data
- snapshot transactions reading data do not block other transactions from writing data
- transactions writing data do not block snapshot transactions from reading data
- If no waiting is acceptable for the SELECT operation but the last committed data is enough to be displayed, this isolation level may be appropriate.