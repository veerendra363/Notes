# Normalization

>Normalization is the process of effectively designing a database such that we can minimize data redundancy(Repetition of similar data).

## Problems without Normalization
1. Insertion Anomaly
2. Deletion Anomaly
3. Updation/Modification Anomaly

**Student Table**  
|rollo|name|brach|hod|office_tel|
|-----|----|-----|---|----------|
|1    |A   |CSE  |X  |12345     |
|2    |B   |CSE  |X  |12345     |
|3    |C   |CSE  |X  |12345     |

- In above table branch details are repeating with each student data(*redundant data*)
- **Insertion Anomaly:** To insert redundant data for every new student is a data insertion problem or anomaly.
- **Deletion Anomaly:** Loss of a related dataset when some other dataset is deleted. In above table if we deleted all students then we are going to loss the branch data also.
- **Updation Anomaly:** Updating data in one place leads to inconsistencies in other parts of the database. If HOD X leaves and Y join as the new HOD of CSE then admin have to update all rows. By any chance few rows are missed while updating then its lead to data inconsistency(CSE brach will have two HODs).




## Levels of Normalization
- [X] 1NF - First Normal Form
- [X] 2NF - Second Normal Form
- [X] 3NF - Third Normal Form
- [ ] BCNF/3.5NF - Boyce-Codd Normal Form
- [ ] 5NF - Fifth Normal Form (PJNF - Project Join Normal Form)
- [ ] 6NF - Sixth Normal Form

**Golden standard of Normalization is 3NF. Most of the companies apply only first Normal Forms.**  

### 1NF
* Each column should contain atomic values.  
* A column should contain values that are of the same type.
* Each column should have unique name.
* Order in which data is saved doesn't matter.

### 2NF
* Must be in 1NF.  
* It should not have any partial dependencies.    

### 3NF
* Must be in 2NF.  
* It should not have any Transitive dependencies.  

### BCNF
* Must be in 3NF.  
* need to write def.  

### 4NF
* Must be in BCNF.  
* It should not have Multi-valued dependency.  

### 5NF/PJNF
* Must be in 4NF
* It should not have join dependency.



>As the data requirement increases, Database complexity increases, and the need for normalization too increase.



