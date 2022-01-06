#   Sampling

    When the data volume is extra large, we may need to find a subset of data to speed up data analysis.
    This is sampling, a technique used to identify and analyze a subset of data in order to discover patterns and trends in the whole dataset.
    In HQL, there are three ways of sampling data: random sampling, bucket table sampling, and block sampling.

#   Random sampling

    Random sampling uses the rand function and LIMIT keyword to get the sampling of data.
    
    SELECT name FROM employee_hr
    DISTRIBUTE BY rand() SORT BY rand() LIMIT 2;

#   Bucket table sampling
    This is a special sampling method, optimized for bucket tables.
    
    The SELECT clause specifies the columns to sample data from.

    The rand function can also be used when sampling entire rows.
    
    If the sample column is also the CLUSTERED BY column, the sample will be more efficient.

#   Sampling based on the whole row

    SELECT name FROM employee_trans
    TABLESAMPLE(BUCKET 1 OUT OF 2 ON rand()) a;

#   Sampling based on the bucket column, which is efficient
    
    SELECT name FROM employee_trans
    TABLESAMPLE(BUCKET 1 OUT OF 2 ON emp_id) a;

#   Block sampling
    
    This type of sampling allows a query to randomly pick up n rows of data, n percentage of the data size, or n bytes of data.
    
    The sampling granularity is the HDFS block size.
#   Sample by number of rows
    SELECT name
    FROM employee TABLESAMPLE(1 ROWS) a;

#   Sample by percentage of data size
    SELECT name
    FROM employee TABLESAMPLE(50 PERCENT) a;

#   Sample by data size and Support b/B, k/K, m/M, g/G
    SELECT name FROM employee TABLESAMPLE(1B) a;
    





