## File System vs DBMS
Everything that can be done using a DBMS can theoretically be implemented using a file system. 

However, doing so would require significant effort to build features such as indexing, querying, concurrency control, transactions, security, and recovery mechanisms. 

A DBMS provides these capabilities out of the box.

### What does DBMS provide ?
#### 1. Querying
Querying is much faster in DBMS as it uses indexing, data structures and algorithms like B Trees, internally to retrieve the data efficiently.

If we consider a file system own searching mechanism needs to be implemented, otherwise finding data would require scanning a larger part of the file 
#### 2. Redundancy
DBMS s support normalization and relational modeling, which help reduce data redundancy and improve storage efficiency.

File system can contain same data duplicated across over multiple files making maintenance difficult.
#### 3. Data Integrity
DBMS offers data integrity through integrity constraints such primary, foreign keys, unique constraints and checks etc. 

For example, an order can reference a payment only if that payment record exists.

But file system doesn't provide this integrity and needs more complex code to be written to support this.
#### 4. Consistency
DBMS offers consistency through transactions, rollbacks etc. to maintain the data in a correct state at any point of time either it be concurrent access or system failure.

This is not feasible in the file system and would require a lot of effort to implement and error-prone.
##### 5. Abstraction and Data Independence
DBMS offers data independence meaning without knowing any internal working of DBMS we can efficiently store, retrieve data.
##### 6. Security and Access control
DBMS s provide fine-grained security mechanisms such as authentication, authorization, roles, privileges, row-level security, and auditing.

File systems generally provide security at the operating system level, but they do not offer the same level of data-specific access control and management as a DBMS.

## DBMS
A DBMS is software that manages databases and provides an organized, efficient, and secure way to store and access data.
Examples of DBMS include MySQL, PostgreSQL, Oracle Database, and MongoDB.

### Database
A database is an organized collection of data that can be efficiently stored, managed, and retrieved.
#### Types of Database
There are multiple types of databases to fulfill various purposes. Some of the widely known DBMS s are Relational, Document, Column, Graph databases etc.
1. Relational Databases: 
	1. Stores data in tables consisting of rows and columns.
	2. MySQL, PostgreSQL
2. Document Databases: 
	1. Stores data in the form of documents JSON/BSON.
	2. MongoDB, 
3. Key Value Databases:
	1. Stores data in the form of key value pairs
	2. DynamoDB, Redis
4. Column Family Databases: 
	1. Store data in column families rather than rows.
	2. Cassandra
5. Graph Databases: 
	1. Store data as nodes and relationships.
	2. Neo4j