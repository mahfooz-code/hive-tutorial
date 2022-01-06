#   CUBE
    The CUBE statement takes a specified set of grouping columns and creates aggregations for all of their 
    possible combinations.

    If n columns are specified for CUBE, there will be 2^n combinations of aggregations returned.
    For example, GROUP BY a,b,c WITH CUBE is equivalent to GROUP BY a,b,c GROUPING SETS ((a,b,c),(a,b),(b,c),(a,c),(a),(b),(c),()).


#   GROUP_ID function

    The GROUPING__ID function works as an extension to distinguish entire rows from each other.

    It returns the decimal equivalent of the BIT vector for each column specified after GROUP BY.

    The returned decimal number is converted from a binary of ones and zeros, which represents whether the column is aggregated (0) in the row or not (1).

    SELECT name, start_date, count(employee_id) as emp_id_cnt,
    GROUPING__ID,
    grouping(name) as gp_name,
    grouping(start_date) as gp_sd
    FROM employee_hr
    GROUP BY name, start_date
    WITH CUBE ORDER BY name, start_date;


