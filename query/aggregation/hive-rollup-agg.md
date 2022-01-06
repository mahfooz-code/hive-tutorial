#   ROLLUP

    The ROLLUP statement enables a SELECT statement to calculate multiple levels of aggregations across a specified 
    group of dimensions.

    The ROLLUP statement is a simple extension of the GROUP BY clause with high efficiency and minimal overhead 
    for a query.

    Compared to GROUPING SETS, which creates specified levels of aggregations, ROLLUP creates n+1 levels of 
    aggregations, where n is the number of grouping columns.

    First, it calculates the standard aggregate values specified in the GROUP BY clause.
    Then, it creates higher-level subtotals, moving from right to left through the list of combinations of grouping columns.
    
    For example, GROUP BY a,b,c WITH ROLLUP is equivalent to GROUP BY a,b,c GROUPING SETS ((a,b,c),(a,b),(a),())

#   
    SELECT name, start_date, GROUPING__ID, count(*)
    FROM employee_hr
    GROUP BY name, start_date WITH ROLLUP;


    SELECT name, start_date, GROUPING__ID,
    grouping(name, start_date), grouping(start_date,name), grouping(name), grouping(start_date),
    count(*)
    FROM employee_hr
    GROUP BY name, start_date WITH ROLLUP;


    
