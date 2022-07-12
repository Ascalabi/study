# DynamoDB
Is a fully managed NoSQL database service. It uses Dynamo model in the essence of its design.

Allows users to create databases capable of storing and retreiving any amount of data, and serving any amount of traffic. It automatically distributes data and traffic over servers to dynamically manage each customer's request.



## Basics
 - **Partition** (hash) key (PK) defines in which portion the data is stored. You can only query by the exact value. 
 - **Sort** (range) key (SK) defines the sortin of items in the partition, optional, combined with the partition key, it defines the primary key of the item. Can query with condition expressions (`=`, `<`, `>`, `between`, `begins_with`)
 - **Local secondary index** (LSI) allows deifining another sort key for the same partition key. It offers consistent reads, up to 5 SLI on a table
 - **Global secondary index** (GSI) allows defining a new partition and optional sort key. It should be preffered over **LSI** except when you need consistent reads. Up to 20 GSI for table. **The central part of most design patterns in a singletbale design**.

- **Attributes** can be scalar (single value) or complex (list, maps, sets) 

- DynamoDB query can return of maximum of 1 MB of results

## Denormalization
Normalization is a standard technique in designing RDBMS. Oganazing data that prevents duplication and enforces consistency. Its achieved by splitting data into mutliple tables. Requires joins.

In DynamoDB data is partitioned. The data isn't stored in a form that is already prepared for our access patters. This is done by denormalization adn duplication. Even secondary indeces are d by design data duplication. They do not just point to the data; they contain whole or part of the data, depending on the projection.


### Denormalization with a complex attribute pattern
The most simple denormalization is to contain all the data in one item... 

The partition key contains the type of entity (USER) and user ID, which in this case, is email. Why email, not UUID, or similar? Because the user will log in with their email, and we will access data by email. The address is stored as a complex attribute and is not separated into another table or record. There is an additional attribute TYPE that defines the type. In the same table, we will store multiple types. The attribute type does not have a special purpose but can be useful when processing the records.
![[Pasted image 20211221212125.png]]

### Denormalization by duplicating data pattern
The solution is the same as in the previous case. You have an item that also contains related data. But in this case, this data is related to other items, so it needs to be duplicated.

## Relationship patters
Most patterns for modeling a relationship take advantage of the following principals:

- Collocate related data by having the same partition key and different sort key to separate it.
- Sort key allows searching so you can limit which related data you want to read and how much of them (e.g., first 10 records)
- **Swap partition and sort key in GSI so you can query the opposite direction of the relationship.** holy fuck

Be aware of **Avodiing Hot Partitions**

If you need to put ID in a sort key, consider using **KSUID** (K-Sortable Unique Identifier) instead of UUID

BRuuuuuh checkaj ta model:
![[DynamoDB]]

This allows you to:
- Read customer by ID (PK = CUSTOMER#XYQ, SK = CUSTOMER#XYQ)
- Read customer and last 10 orders together (PK = CUSTOMER#XYQ, Limit = 11)
- Read only orders of the customer (PK = CUSTOMER#XYQ, SK = begins_with("ORDER"))
- Read only one order (PK = CUSTOMER#XYQ, SK= ORDER#00001)


Okej tle je same model extended with one to many relationship between order and logs with help of GSI:
[Pac je too long](https://www.serverlesslife.com/DynamoDB_Design_Patterns_for_Single_Table_Design.html) 
.
.
.
.
ya kinda crazy
## Single table design
Offers a rich set of flexible and efficient design patterns, better utilization of resources, easier configuration, capacity planning and more monitoring.

The downside of a single table design is that data looks confusing and hard to read.

**Principals**:
- Put all data in one table or as few as possible
- Reuse keys and secondary indexes for different purposes
- Name keys and secondary indexes with unifrom names (like PK (partition key), SK (sort key), GS1PK (first global secondary index partition key), GS1SK (first global secondary index sort key))
- Duplicate and separate calues for keys, indexes from actual application attributes. That means that you can have the same values in two, or more places.
- Add a special attribute to define the type. So its more readable
- Do not store too much data in one record (400KB limit)
- Use the partition key that is actually used in access patterns. For example, for user use username or email, not some random ID. This way, you ke advatage of the primary index, which otherwise would be unused.
