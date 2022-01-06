#   Performnace
    HQL provides the EXPLAIN and ANALYZE statements, which can be used as utilities to check and identify the performance of queries. 


#   EXPLAIN

    Hive provides an EXPLAIN statement to return a query execution plan without running the query.
    We can use it to analyze queries if we have concerns about their performance.
    The EXPLAIN statement helps us to see the difference between two or more queries for the same purpose.

    EXPLAIN [FORMATTED|EXTENDED|DEPENDENCY|AUTHORIZATION] hql_query

#   FORMATTED: 
    This provides a formatted JSON version of the query plan.

#   EXTENDED: 
    This provides additional information for the operators in the plan, such as file pathname.

#   DEPENDENCY: 
    This provides a JSON format output that contains a list of tables and partitions that the query depends on. It has been available since Hive v0.10.0

#   AUTHORIZATION: 
    This lists all entities needed to be authorized, including input and output to run the query, and authorization failure, if any. It has been available since Hive v0.14.0.


#   A typical query plan contains the following three sections.

#   Abstract Syntax Tree (AST):
    
    Hive uses a parser generator called ANTLR (http://www.antlr.org/) to automatically generate a tree syntax for HQL.
    
#   Stage Dependencies: 
    
    This lists all dependencies and the number of stages used to run the query

#   Stage Plans: 
    
    It contains important information, such as operators and sort orders, for running the job


#   explain query plan

    EXPLAIN SELECT gender_age.gender, count(*)
    FROM employee_partitioned WHERE year=2018
    GROUP BY gender_age.gender LIMIT 2;


#   ANALYZE

    Hive statistics are a collection of data that describes more details, such as the number of rows, number of files, 
    and raw data size of the objects in the database.
    Statistics are the metadata of data, collected and stored in the metastore database.
    Hive supports statistics at the table, partition, and column level. These statistics serve as an input to the Hive Cost-Based Optimizer (CBO), which is an optimizer used to pick the query plan with the lowest cost in terms of system resources required to complete the query.

    The statistics are partially gathered automatically in Hive v3.2.0 through to JIRA HIVE-11160 or manually through the ANALYZE statement on tables, partitions, and columns.

#   Collect statistics on the existing table.
    
    When the NOSCAN option is specified, the command runs faster by ignoring file scanning but only collecting the number of files and their size.

    ANALYZE TABLE employee COMPUTE STATISTICS;

#   Collect statistics on specific:

    ANALYZE TABLE employee_partitioned
    PARTITION(year=2018, month=12) COMPUTE STATISTICS;

#   Applies for all partitions
    ANALYZE TABLE employee_partitioned
    PARTITION(year, month) COMPUTE STATISTICS;

#   Collect statistics on columns for existing tables.
    
    ANALYZE TABLE employee_id COMPUTE STATISTICS FOR COLUMNS employee_id;

#   Automatic stats gathering
    We can enable automatic gathering of statistics by specifying 
    
    SET hive.stats.autogather=true
    
    For new tables or partitions that are populated through the INSERT OVERWRITE/INTO statement (rather than the LOAD statement), statistics are automatically collected in the metastore.

#   Check statistics in a table
    DESCRIBE EXTENDED employee;

#   Check statistics in a partition
    DESCRIBE EXTENDED employee_partitioned PARTITION(year=2018, month=12);

#   Check statistics in a column
    DESCRIBE FORMATTED employee.name;

    




