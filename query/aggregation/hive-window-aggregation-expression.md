#   Window expression
    
    [<window_expression>] is used to further sub-partition the result and apply the window functions.
    There are two types of windows: 
    Row Type and Range Type.

#   Range Type

    SELECT name, dept_num as dno, salary as sal,
    max(salary) OVER (PARTITION BY dept_num ORDER BY name
    ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) win1,
    max(salary) OVER (PARTITION BY dept_num ORDER BY name 
    ROWS BETWEEN 2 PRECEDING AND UNBOUNDED FOLLOWING) win2,
    max(salary) OVER (PARTITION BY dept_num ORDER BY name
    ROWS BETWEEN 1 PRECEDING AND 2 FOLLOWING) win3,
    max(salary) OVER (PARTITION BY dept_num ORDER BY name
    ROWS BETWEEN 2 PRECEDING AND 1 PRECEDING) win4,
    max(salary) OVER (PARTITION BY dept_num ORDER BY name
    ROWS BETWEEN 1 FOLLOWING AND 2 FOLLOWING) win5,
    max(salary) OVER (PARTITION BY dept_num ORDER BY name
    ROWS 2 PRECEDING) win6, -- FOLLOWING does not work in this way
    max(salary) OVER (PARTITION BY dept_num ORDER BY name
    ROWS UNBOUNDED PRECEDING) win7
    FROM employee_contract
    ORDER BY dno, name;


#   Row Type

