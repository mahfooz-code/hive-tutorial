#   Group set

    GROUPING SETS implements advanced multiple GROUP BY operations against the same set of data.
    The GROUPING SETS clause in GROUP BY allows us to specify more than one GROUP BY option in the same record set.
     All GROUPING SET clauses can be logically expressed in terms of several GROUP BY queries connected by UNION.
    Actually, GROUPING SETS are a shorthand way of connecting several GROUP BY result sets with UNION.
    The GROUPING SETS keyword completes all processes in a single stage of the job, which is more efficient.
    A blank set in the GROUPING SETS clause calculates the overall aggregation.
    For better understanding, we can say that the outer level (brace) of GROUPING SETS defines what data UNION is to be implemented.

#   Grouping set with one element of column pairs.
    
    SELECT name, start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY name, start_date
    GROUPING SETS((name, start_date));
    
    SELECT name, start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY name, start_date;

#   Grouping set with two elements.

    SELECT name, start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY name, start_date
    GROUPING SETS(name, start_date);

    Equivalent query:

    SELECT name, null as start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY name

    UNION

    SELECT null as name, start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY start_date;

#   Grouping set with two elements, a column pair, and a column.

    SELECT name, start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY name, start_date
    GROUPING SETS((name, start_date), name);

    Equivalent query:

    SELECT name, start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY name, start_date
    UNION 
    SELECT name, null as start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY name;



#   Grouping set with four, including all combinations of columns.

    select name, start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY name, start_date
    GROUPING SETS((name, start_date), name, start_date, ());

    Equivalent query:

    SELECT name, start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY name, start_date

    UNION 

    SELECT name, null as start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY name

    UNION 

    SELECT null as name, start_date, count(sin_number) as sin_cnt
    FROM employee_hr
    GROUP BY start_date

    UNION 

    SELECT null as name, null as start_date, count(sin_number) as sin_cnt
    FROM employee_hr

#   Group set function
    When aggregates are displayed for a column its value is null.
    This may conflict in case the column itself has some null values.
    There needs to be some way to identify NULL in column, which means aggregate and NULL in column, which means value. GROUPING__ID function is the solution to that.

    This function returns a bitvector corresponding to whether each column is present or not.
    For each column, a value of "1" is produced for a row in the result set if that column has been aggregated in that row, otherwise the value is "0".
    This can be used to differentiate when there are nulls in the data.




