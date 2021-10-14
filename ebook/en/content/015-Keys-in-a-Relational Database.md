# Relational Keys- Keys in a Relational Database

A database must be able to inhibit inconsistency occurring due to incorrect data. It must have certain identified attributes in relations to uniquely distinguish the tuples. No two tuples in a relation should have same value for all attributes since it will lead to duplicity of data. duplicity of data leads to inconsistency . Relational database systems have the concept of Relational Keys to distinguish between different records.   

## Types of Relational Keys
### *Super Keys*

A relation’s tuples can be uniquely identified by various combinations of attributes. Super Keys is defined as a set of one attribute or combinations of two or more attributes that help in distinguishing between tuples in a relation.

For example, the Customer ID attribute of the relation Customer is unique for all customers. The Customer ID can be used to identify each customer tuple in the relation. Customer ID is a Super Key for relation Customer.

Customer Name attribute of Customer cannot be considered as Super Key because many customers for the organization can have same name. However when combined with Customer ID it becomes a Super Key {CustomerID, CustomerName}. It means that Super Key can have additional attributes.  Consider any key K which is identified as a super key. Any superset of key K is also a super key. For example the possible Super Keys for Customer Relation are

- [CustomerID, CustomerName, Customer Address]
- [CustomerID, CustomerName, Customer Contact Number]
- [CustomerID, Customer Contact Number]

### *Candidate Keys*

If we take a key from the set of super keys for which we don’t have any  proper subset defined as a superkey, it is called a candidate key. In other words the minimal attribute super keys are termed as candidate keys.

If we can identify some distinct sets of attributes which identify the tuples uniquely they fall in the category of candidate keys. For example the possible Candidate Keys for Customer Relation are

- [ CustomerID ]
- [ CustomerName, Customer Address ]
- [ CustomerName, Customer Contact Number ]
- [ Customer Address, Customer Contact Number ]

### *Primary Key*

Out of all possible candidate keys only one is chosen by the database designer as the key to identify the records in a relation in a database. This selected candidate key is called the Primary Key. It is the property of the relation and not of tuples. The primary key attribute(s) does not allow any duplicate values. It also inhibits leaving the primary key attribute without any value (NOT NULL).

**A relation can have only one primary key.**

In the Customer Database example {Customer ID} is the attribute taken as the primary key of customer relation. While picking up a candidate key as primary key the designer should ensure that it is an attribute or group of attributes that do not change or may change extremely rarely.

### *Alternate Keys*

After selecting one key among candidate keys as primary key, the rest of candidate keys are called the alternate keys. In the customer Database these candidate keys are the alternate keys.

- [ CustomerName, Customer Address ]
- [ CustomerName, Customer Contact Number ]
- [ Customer Address, Customer Contact Number ]

### *Foreign Key*

A foreign key is used to reference values from one relation into another relation. This is possible when the attribute or combination of attributes is primary key in the referenced relation. The relation in which the primary key of a relation is referenced is called the referencing table. The foreign key constraint implements the referential integrity in a database. The referencing relation attribute can have only those values which exist in the primary key attribute(s) of the referenced relation

**A relation can have multiple foreign key**

For example in the customer database the orders' relation (referencing relation) has the structure (Order ID, Customer ID, Order Date, Order Status, Total Billing Amount). The attribute Customer ID is the foreign key referencing Customer ID from customer relation (referenced relation). It means that orders can be placed only for the customers whose customer details are already available in the customer relation.