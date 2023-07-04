[<--](./index.md)
# DBMS Keys

> A DBMS key is an attribute or a set of attribute which help you uniquely identify a record or a row of data in a relation(table)

## Types of DBMS keys

1. Super key
2. Candidate key
3. Primary key
4. Alternate key
5. Foreign key
6. Composite key
7. Compound key
8. Surrogate key
9. Natural key
10. Unique key

**Student Table**  
Note: every student have email  
|S_Id|S_Name|B_Code|S_Email |
|----|------|------|--------|
|1   |Veeru |CS    |v@cs.com|

**Branch Table**
|B_Code|B_HOD|B_tel|
|------|-----|-----|
|CS    |Hari |12345|

### Super key
The attribute or combination of attributes that can uniquely identify a row is known as super key.  

Super keys in student table are S_Id, S_Email, S_Id + S_Name, S_Id + B_Code, S_Id + S_Email, S_Email + S_Name, S_Email + B_Code, S_Id + S_Name + B_Code, .....

### Candidate Key
The attribute or minimal set of attributes that can uniquely identify a row is known as candidate key.  

Candidate Keys in student table are S_Id, S_Email.

### Primary Key
The attribute or minimal set of attribute that can uniquely identify a row and a table can consist of just one primary key.
Admin select one candidate key as primary key.

We will take S_Id as primary key from student table.

### Alternate key
Except primary key all remaining all keys are alternate keys.

Alternate key in student table is S_Email.

>Candidate keys = Primary key + Alternate keys

### Foreign Key
A foreign key establishes a relationship between two tables by referencing the primary key of another table.

Foreign key in student table is B_Code.

### Composite key
The combination of attributes that can uniquely identify a row is called as Composite key.

Composite keys in the student table are S_Id + S_Email, S_Id + S_Email + S_Name, S_Id + B_Code.....

### Compound key
If a composite key has at-least one attribute which is a foreign key then it is called as compound key.

Compound keys in the student table are S_Id + B_Code, S_Email + B_Code, ....

### Surrogate key
The system-generated unique identifier used to identify the rows uniquely in the table are called Surrogate key

Surrogate key example is id column in each table.

### Natural key
A natural key is a unique identifier in a database table that is derived from inherent and meaningful data attributes or elements.

Example: SSN (social security number), Aadhaar Number, some times phone number and email.

### Unique key
A unique key is a database constraint that ensures that a specific column or combination of columns contains only unique values

Unique key in the student table are S_Id, S_Email.






