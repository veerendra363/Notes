# Leetcode SQL 50
[Link to SQL 50](https://leetcode.com/studyplan/top-sql-50/)

I solved the problems in this using mysql

### Learning

#### Null in where caluse
!= condition is not applied to null  
for example: num != 1 it returns all rows excpet row where num with null  
if you want to include null as well add the is null condition also  
num != 1 or num is null  
null always returns unknow when used with basic oprators like =, !=, >, <  
but where returns only true rows  
so be careful with null columns  


#### Date & Date functions
date_format(date, format)  
current_date() / curdate()  
current_time() / curtime()  
now() / localtime() /current_timestamp()  
date_add(date,  interval value unit)  
addDate(date, interval value unit)  
date_sub() subdate()  
addtime(exp1, exp2)  
subtime(exp1, exp2)  
datediff(date1, date2)  
year(date)  
month(date)  
day(date)  

we can compare the date using >, >=, <, <= and between in where conditon

#### aggregate functions
count()  
sum()  
avg()  
max()  
min()  
group_concat()  

Group By  
Having  

#### window functions
all startred aggreate function are used as window functions like sum, avg, max, min and count  
rank()  
dense_rank()  
row_number()  

OVER ([PARTITION BY expr1 [, expr2, ...]]
      [ORDER BY expr3 [ASC | DESC] [, expr4 [ASC | DESC], ...]]
      [ROWS | RANGE BETWEEN frame_start AND frame_end])
      
-------------
limit and offset
joins  
case when condition 1 val then  
    when condition 2 val then  
..... else val end  
if ()  
ifnull(val, default value)




