#   Rank 

    SELECT a1.Name, a1.Sales, COUNT (a2.Sales) Sales_Rank
    FROM Total_Sales a1, Total_Sales a2
    WHERE a1.Sales < a2.Sales OR (a1.Sales=a2.Sales AND a1.Name = a2.Name)
    GROUP BY a1.Name, a1.Sales
    ORDER BY a1.Sales DESC, a1.Name DESC;

#   median
    To get the median, we need to be able to accomplish the following:

    Sort the rows in order and find the rank for each row.
    Determine what is the "middle" rank. For example, if there are 9 rows, the middle rank would be 5.
    Obtain the value for the middle-ranked row.

    SELECT Sales Median FROM
    (SELECT a1.Name, a1.Sales, COUNT(a1.Sales) Rank
    FROM Total_Sales a1, Total_Sales a2
    WHERE a1.Sales < a2.Sales OR (a1.Sales=a2.Sales AND a1.Name <= a2.Name)
    group by a1.Name, a1.Sales
    order by a1.Sales desc) a3
    WHERE Rank = (SELECT (COUNT(*)+1) DIV 2 FROM Total_Sales);


#   running total
    SELECT a1.Name, a1.Sales, SUM(a2.Sales) Running_Total
    FROM Total_Sales a1, Total_Sales a2
    WHERE a1.Sales <= a2.sales or (a1.Sales=a2.Sales and a1.Name = a2.Name)
    GROUP BY a1.Name, a1.Sales
    ORDER BY a1.Sales DESC, a1.Name DESC;


    select companyintegerid, 
    invoicenumber, 
    count(*) over (partition by invoicenumber order by invoicenumber asc) as cust_count 
    from actives_raw_daily where partition_date >= '2021-08-02'

#   Hadoop Hive analytic functions compute an aggregate value that is based on a group of rows.
    A Hadoop Hive HQL analytic function works on the group of rows and ignores the NULL in the data if you specify.

#   Hadoop Hive COUNT Analytic Function
    Returns number of rows in query or group of rows.

    Syntax:

    COUNT(column reference | value expression | *) over(window_spec)

    select pat_id, 
    dept_id, 
    count(*) over (partition by dept_id order by dept_id asc) as pat_cnt 
    from patient;


