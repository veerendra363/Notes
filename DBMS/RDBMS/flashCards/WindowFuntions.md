[<<<](../index.md)

## Window Functions
> window - set of rows.  

over( partition by col order by col).  
we will use aggregate function and window functions with windows.  

window functions:  
1. row_number()
2. rank()
3. dense_rank()
4. lag(col, offset_optional, default_value__optional)
5. lead(col,  offset_optional, default_value_optional)
6. first_value(col)
7. last_value(col)
8. nth_value(col, n)
9. ntile(n)
10. cume_dist()
11. percent_rank()

> Frame - set of rows and subset of window

The FRAME clause has two main options:
1. rows
2. range  

```
rows between 2 preceding and 2 following
range between unbounded preceding and unbounded following
```
