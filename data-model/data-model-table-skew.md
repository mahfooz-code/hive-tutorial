#   Skew table
    
    Besides regular internal/external or partition tables, we should also consider using a skewed 
    or temporary table for better design as well as performance.

    Since Hive v0.10.0, HQL has supported the creation of a special table for organizing skewed data.
    A skewed table can be used to improve performance by splitting those skewed values into separate files 
    or directories automatically.
    
    As a result, the total number of files or partition folders is reduced.
    Also, a query can include or ignore this data quickly and efficiently.

#   Creating skew table

    CREATE TABLE sample_skewed_table (
        dept_no int,
        dept_name string
    )
    SKEWED BY (dept_no) ON (1000, 2000);

#   Describe the skew table
    DESC FORMATTED sample_skewed_table;
