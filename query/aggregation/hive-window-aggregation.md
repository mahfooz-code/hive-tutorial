#   Window

    Window functions, available since Hive v0.11.0, are a special group of functions that scan multiple input rows to compute each output value.

    Window functions are usually used with OVER, PARTITION BY, ORDER BY, and the windowing specification.

    Different from the regular aggregate functions used with the GROUP BY clause, and limited to one result value per group, window functions operate on windows where the input rows are ordered and grouped using flexible conditions expressed through an OVER and PARTITION clause. 

    Window functions give aggregate results, but they do not group the result set.

#   Syntax
    
    The syntax for a window function is as follows:
    Function (arg1,..., argn) OVER ([PARTITION BY <...>] [ORDER BY <....>] [<window_expression>])

    Function (arg1,..., argn) can be any function in the following four categories:
    
    Aggregate Functions: Regular aggregate functions, such as sum, and max.
    
    Sort Functions: Functions for sorting data, such as rank, androw_number.
    
    Analytics Functions: Functions for statistics and comparisons, such as lead, lag, and first_value.


#   Window aggregate functions

#   Prepare the table and data for demonstration:

    CREATE TABLE IF NOT EXISTS employee_contract (
        name string,
        dept_num int,
        employee_id int,
        salary int,
        type string,
        start_date date
    )
    ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '|'
    STORED as TEXTFILE;

#   Loading data into table
    LOAD DATA LOCAL INPATH '/home/malam/employee_contract.txt'
    OVERWRITE INTO TABLE employee_contract;

#   The regular aggregations are used as window functions.

    SELECT name, dept_num as deptno, salary,
    count(*) OVER (PARTITION BY dept_num) as cnt,
    count(distinct dept_num) OVER (PARTITION BY dept_num) as dcnt,
    sum(salary) OVER(PARTITION BY dept_num ORDER BY dept_num) as sum1,
    sum(salary) OVER(ORDER BY dept_num) as sum2,
    sum(salary) OVER(ORDER BY dept_num, name) as sum3
    FROM employee_contract
    ORDER BY deptno, name;


#   Window sort functions

    Window sort functions provide the sorting data information, such as row number and rank, within specific groups as part of the data returned. 

    The most commonly used sort functions are as follows:

#   row_number: 
    Assigns a unique sequence number starting from 1 to each row, according to the partition and order specification.

#   rank: 
    
    Ranks items in a group, such as finding the top N rows for specific conditions.

#   dense_rank: 
    Similar to rank, but leaves no gaps in the ranking sequence when there are ties. 
    
    For example, if we rank a match using dense_rank and have two players tied for second place, we would see that the two players were both in second place and that the next person is ranked third.
    
    However, the rank function would rank two people in second place, but the next person would be in fourth place.

#   percent_rank: 
    Uses rank values rather than row counts in its numerator as (current rank - 1)/(total number of rows - 1). 
    
    Therefore, it returns the percentage rank of a value relative to a group of values.

#   ntile: 
    Divides an ordered dataset into a number of buckets and assigns an appropriate bucket number to each row.
    
    It can be used to divide rows into equal sets and assign a number to each row.

#   window sort functions in HQL

    SELECT name, dept_num as deptno, salary,
    row_number() OVER () as rnum, -- sequence in orginal table 
    rank() OVER (PARTITION BY dept_num ORDER BY salary) as rk,
    dense_rank() OVER (PARTITION BY dept_num ORDER BY salary) as drk,
    percent_rank() OVER(PARTITION BY dept_num ORDER BY salary) as prk,
    ntile(4) OVER(PARTITION BY dept_num ORDER BY salary) as ntile
    FROM employee_contract
    ORDER BY deptno, name;

#   Since Hive v2.1.0, we have been able to use agg on OVER
    
    SELECT dept_num, 
    rank() OVER (PARTITION BY dept_num ORDER BY sum(salary)) as rk
    FROM employee_contract
    GROUP BY dept_num;


#   Window analytics functions

    Window analytics functions provide extended data analytics, such as getting lag, lead, last, or first rows in the ordered set.
    

#   cume_dist: 
    Computes the number of rows whose value is smaller than or equal to, the value of the total number of rows divided by the current row, such as (number of rows ≤ current row)/(total number of rows).

#   lead: 
    This function, lead(value_expr[,offset[,default]]), is used to return data from the next row. The number (offset) of rows to lead can optionally be specified, one is by default. The function returns [,default] or NULL when the default is not specified. In addition, the lead for the current row extends beyond the end of the window.

#   lag: 
    This function, lag(value_expr[,offset[,default]]), is used to access data from a previous row. The number (offset) of rows to lag can optionally be specified, one is by default. The function returns [,default] or NULL when the default is not specified. In addition, the lag for the current row extends beyond the end of the window.

#   first_value: 
    It returns the first result from an ordered set.

#   last_value: 
    It returns the last result from an ordered set. 


#   analytical function

    SELECT name,dept_num as deptno,salary,
    cume_dist() OVER (PARTITION BY dept_num ORDER BY salary) as cume,
    lead(salary, 2) OVER (PARTITION BY dept_num ORDER BY salary) as lead,
    lag(salary, 2, 0) OVER (PARTITION BY dept_num ORDER BY salary) as lag,
    first_value(salary) OVER (PARTITION BY dept_num ORDER BY salary) as fval,
    last_value(salary) OVER (PARTITION BY dept_num ORDER BY salary) as lval,
    last_value(salary) OVER (PARTITION BY dept_num ORDER BY salary RANGE BETWEEN UNBOUNDED
    PRECEDING AND UNBOUNDED FOLLOWING) as lval2
    FROM employee_contract
    ORDER BY deptno, salary;










