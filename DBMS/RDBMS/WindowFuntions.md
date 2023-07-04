[<<<](./index.md)

## Window Functions
Window functions applies aggregate and ranking functions over a particular window (set of rows). OVER clause is used with window functions to define that window. OVER clause does two things : 

Partitions rows into form set of rows. (PARTITION BY clause is used) 
Orders rows within those partitions into a particular order. (ORDER BY clause is used) 
Note: If partitions aren’t done, then ORDER BY orders all rows of table. 

### Syntax
```sql
SELECT column_name1, 
 window_function(column_name2)
 OVER([PARTITION BY column_name1] [ORDER BY column_name3]) AS new_column
FROM table_name;
```

**Employee Table**
|id|name|dept|salary|
|--|----|----|------|
|1|a|IT|1000|
|2|b|QA|500|
|3|c|Sales|2000|
|4|d|IT|1500|
|5|e|IT|1500|
|6|f|QA|200|
|7|g|Admin|3000|
|8|h|QA|7000|

**row_number():** It assigns unique number to each record in the window.

**rank():** It assigns a unique rank to each row within a result set based on the specified ordering. Rows with the same values will receive the same rank, and the next rank will be skipped.

**dense_rank()** It is same like rank but doesn't skip ranks.

```sql
select *,
row_number() over (partition by dept) as rn,
rank() over (partition by dept order by salary desc) as rnk,
dense_rank() over (partition by dept order by salary) as dense_rnk,
from employee;
```
**lag(col, offset, default_value):** The Lag function allows you to retrieve data from the previous row in a result set. It takes three arguments: the column you want to retrieve, the number of rows back to look, and an optional default value if the requested row is not available. The Lag function is useful for comparing values between current and previous rows.

**lead(col, offset, default_value):** The Lead function is similar to the Lag function but retrieves data from the next row in a result set. It also takes three arguments: the column to retrieve, the number of rows forward to look, and an optional default value.

```sql
select *,
lag(salary) over (partition by dept order by id) as prev_emp_salary,
lead(salary, 2, 0) over (partition by dept order by id) as second_next_emp_salary
from employee
```

**first_value(col):** It will extract the first value of the given columns from each window.

**last_value(col)** It will extract the last value of the given columns from each window.

**nth_value(col, n)** It will extract the nth value of the given columns from each window.


> Frame - set of rows. frame is subset of the window.

**Frame Clause:**  The FRAME clause is used in conjunction with window functions to define the window or subset of rows within the partition to which the function is applied.

The FRAME clause has two main options:
1) ROWS: It defines the window frame based on the number of rows. There are three variations:
- UNBOUNDED PRECEDING: Includes all rows from the start of the partition up to and including the current row.
- n PRECEDING: Includes the current row and the n preceding rows.
- BETWEEN n PRECEDING AND m FOLLOWING: Includes the current row and the n preceding rows, as well as the m following rows.
2) RANGE: It defines the window frame based on the values of the ORDER BY expression. It considers the logical ordering of rows rather than their physical position. The range can be defined using different variations:
- UNBOUNDED PRECEDING: Includes all rows from the start of the partition up to and including the current row based on the ordering of values.
- n PRECEDING: Includes the current row and the rows whose values are within n units away from the current row.
- BETWEEN n PRECEDING AND m FOLLOWING: Includes the current row and the rows whose values are within the range of n units before and m units after the current row.

`
range between unbounded preceding and current row
rows between unbounded preceding and current row

range between unbounded preceding and unbounded following
rows between unbounded preceding and unbounded following

range between 2 preceding and 2 following
rows between 2 preceding and 2 following
`

```sql
select *,
first_value(name)
    over(partition by dept order by salary desc
    range between 2 preceding and 2 following) 
    as first_salary_employee_name,
last_value(name)
    over(partition by dept order by salary desc
    range between 2 preceding and 2 following) 
    as last_salary_employee_name,
nth_value(name, 2)
    over(partition by dept order by salary desc
    range between 2 preceding and 2 following) 
    as 2nd_salary_employee_name
from employee
```
**Alternate way of writing window queries**
```sql
select *,
first_value(name) over w as first_salary_employee_name,
last_value(name) over w as last_salary_employee_name,
nth_value(name, 2) over w as 2nd_salary_employee_name
from employee
where dept = 'IT'
window w as (partition by dept order by salary desc
    range between 2 preceding and 2 following) 
```

**ntile(n):** It will group the rows into some buckets.  
```sql
select name,
case when x.buckets = 1 then 'High salary'
     when x.buckets = 2 then 'Mid salary'
     when x.buckets = 3 then 'Least salary' end salary_category
from (
    select *,
    ntile(3) over (order by salary desc) as buckets from Employee ) x;
```

**cume_dis():** cumulative distribution
value --> 1 <= cume_dist() > 0
formula = current row number(or row number with value same as current row) / total no of rows

**percent_rank():** relative rank of the current row / percentage ranking
value --> 1 <= percent_rank >= 0
formula = current row no - 1 / total no of rows - 1


```sql
select *,
cume_dist() over (order by salary desc) as cume_distribution,
round (cume_dist() over (order by salary desc) ::number * 100, 2) as cume_dist_percentage
from Employee
```
