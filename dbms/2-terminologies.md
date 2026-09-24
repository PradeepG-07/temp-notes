### Relational Databases
#### Tables
A table stores the data consisting of columns and rows.
Columns are also called as fields/**attributes** and rows are also called as records/**tuples**.
**Instance**: Set of tuples/rows along with the table structure.
#### Key: 
*Minimum* set of attributes/columns that uniquely identify a tuple/row.
- **Simple Key**: A key with a single attribute
- **Compound Key**: A key with more than a single attribute
- **Candidate Key**: A **candidate key** is any minimal set of attributes that uniquely identifies a tuple (row). A relation may have one or more candidate keys.
	- Uniquely identifies each row.
	- Is minimal (removing any attribute destroys uniqueness).
	- Candidate keys do not contain NULL values in the relational model.
- **Primary key**: One of the candidate keys that the DB designer/DBA choose to maintain uniqueness. Mostly primary key is chosen as ID of the table.
	- The following three points are also called **Entity Integrity**
	- Primary key should always contain a NOT NULL value
	- At most one primary key should be there in a table.
	- Primary key value should be unique for each row/tuple.
- **Alternate/Secondary Key**: Candidate keys which are not primary key
- **Super Key**: A **super key** is any set of attributes that uniquely identifies a tuple (row) in a relation.
	- Properties:
		- Every candidate key is a super key.
		- A super key may contain unnecessary (extra) attributes.
		- A candidate key is a minimal super key.
- **Foreign Key**: A foreign key is a column (or set of columns) in one table that references the primary key (or a unique key) of another table, establishing a relationship between the two tables.
	- It ensures **referential integrity**, meaning a record cannot reference data that does not exist in the related table.
- **Self Referential Foreign Key**: A self-referential foreign key is a foreign key that references the primary key of the **same table**.

```java
Super Keys
│
├── Candidate Keys (minimal super keys)
│     │
│     └── One chosen as the Primary Key
│
└── Non-minimal Super Keys
```
#### Integrity Constraints
Integrity constraints are rules enforced by a database to ensure that data remains accurate, valid, and consistent.
##### 1. Entity Integrity
Entity integrity constraint is a constraint on primary keys.
It says every table must have a primary key, every primary key has to be unique and not null.
##### 2. Domain Integrity
Domain integrity ensures that every column contains only valid values that conform to its defined data type, range, format, and constraints.
##### 3. User Specific Integrity
User-defined integrity consists of business rules and constraints that are specific to an application's requirements
##### 4. Referential Integrity
Referential Integrity constraints are in between **referenced** table and **referencing** table.
**Changes made in referenced table**
1. Insert: No need to change anything in referencing table.
2. Delete: Changes are needed. Multiple options are there as follows
	1. **On Delete No action**: Nothing needs to be done in referencing table. But remember the referential integrity is violated.
	2. **On Delete Cascade**: Delete the tuples/rows in referencing table which corresponds to the foreign key
	3. **On Delete Set Null**: Set the the foreign key column in referencing table with null for all the rows that corresponds to that foreign key. Most widely used
3. Modify/edit: Changes are needed. Multiple options are there as follows
	1. **On Edit No action**: Nothing needs to be done in referencing table. But remember the referential integrity is violated.
	2. **On Edit Cascade**: Edit the tuples/rows in referencing table which corresponds to the foreign key. Most widely used but time consuming.
	3. **On Edit Set Null**: Set the the foreign key column in referencing table with null for all the rows that corresponds to that foreign key.
**Changes made in referencing table**
1. Insert: A check will happen for the value in foreign key column against the referenced table primary key column. If value doesn't exist it will throw an error.
2. Delete: Nothing needs to be done in referenced table.
3. Modify/edit: On modification it will check for any violation meaning, value of updated column in referencing table should exist in the referenced table.
##### On Delete Cascade with self referential foreign keys
1. When a tuple is deleted with some primary key column value, DBMS triggers the deletion of other tuples with that primary key value present in foreign key column.
2. Which will again causes the cascading deletions as there is a new deletion.
#### Relational Schema
Relational schema is table structure along with the integrity constraints.
One of the job of DBA is create schema meaning selecting which all attributes need to be part of the table and integrity constraints enforced on the attributes.


Try to solve a example with a table identifying all the mentioned things over here