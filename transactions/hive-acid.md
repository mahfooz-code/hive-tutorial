#   Hive transaction
    
    ACID (Atomicity, Consistency, Isolation, and Durability) is a long-expected Hive feature, and builds a foundation for relational databases; it has been available since Hive v0.14.0.
    
    Full ACID support in Hive is implemented through row-level transactions and locks.
    This makes it possible for Hive to deal with use cases such as concurrent read/write, data cleaning, data modification, complex ETL/SCD (Slow Changing Dimensions), streaming data ingest, bulk data merge, and so on.

    For now, all transactions in HQL are auto-committed without supporting BEGIN, COMMIT, and ROLLBACK, like as with relational databases.

**Also, the table that has a transaction feature enabled has to be a bucket table with the ORC file format.**

#   Setting transaction configuration
    SET hive.support.concurrency = true;
    SET hive.enforce.bucketing = true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    SET hive.txn.manager = org.apache.hadoop.hive.ql.lockmgr.DbTxnManager;
    SET hive.compactor.initiator.on = true;
    SET hive.compactor.worker.threads = 1;

#   Note
    When a transaction is enabled, each transaction-related operation, such as INSERT, UPDATE, and DELETE, stores data in delta files.
    
    At read time, the reader merges the base and delta files, applying any updates and deletes. 
    
    Both base and delta directory names contain the transaction IDs. Occasionally, these changes need to be merged into the base files by compactors, which are background processes in the metastore, for better performance and smaller file size.
    
    To see a list of all tables/partitions currently being compacted or scheduled for compaction, use the SHOW COMPACTIONS statement.

#   Create transactional table

    CREATE TABLE employee_trans (
        emp_id int,
        name string,
        start_date date,
        quit_date date,
        quit_flag string
    )
    CLUSTERED BY (emp_id) INTO 2 BUCKETS STORED as ORC
    TBLPROPERTIES ('transactional'='true'); 

#   Loading data into table
    INSERT INTO TABLE employee_trans VALUES
    (100, 'Michael', '2017-02-01', null, 'N'),
    (101, 'Will', '2017-03-01', null, 'N'),
    (102, 'Steven', '2018-01-01', null, 'N'),
    (104, 'Lucy', '2017-10-01', null, 'N');

#   Update
    For a table with transactions enabled, we can perform UPDATE, DELETE, and MERGE operations on data.

    The UPDATE statement is used to update one or more columns in a table when certain conditions are met.
    
    UPDATE employee_trans
    SET quite_date = current_date, quit_flag = 'Y'
    WHERE emp_id = 104;

    SELECT quit_date, quit_flag
    FROM employee_trans
    WHERE emp_id = 104;

#   DELETE
    
    The DELETE statement is used to remove one or more rows from a table when a certain condition is met.

    DELETE FROM employee_trans WHERE emp_id = 104;
    
    SELECT name FROM employee_trans WHERE emp_id = 104;

#   MERGE

    The MERGE statement, available since Hive 2.2, is used to perform UPDATE, DELETE, or INSERT on a target table, based on the JOIN condition matching or not against a source table or query. 

    MERGE INTO <target_table> as Target USING <source_query/table> as Source ON <join_condition between two tables>
    WHEN MATCHED [AND <boolean expression>]
        THEN UPDATE SET <set clause list>
    WHEN MATCHED [AND <boolean expression>]
        THEN DELETE
    WHEN NOT MATCHED [AND <boolean expression>]
        THEN INSERT VALUES <value list>

    The current limitations for the MERGE INTO statement are as follows:

    1.  One, two, or three WHEN clauses may be present
    2.  At most one of each type of UPDATE/DELETE/INSERT can be used
    3.  WHEN NOT MATCHED must be the last clause and only supports INSERT VALUES <value_list>
    4.  WHEN MATCHED only supports UPDATE or DELETE
    5.  If both UPDATE and DELETE clauses are present, the first one in the statement must include [AND <boolean expression>]

    CREATE TABLE employee_update (
    emp_id int,
    name string,
    start_date date,
    quit_date date,
    quit_flag string
    );

    INSERT INTO TABLE employee_update VALUES
    (100, 'Michael', '2017-02-01', '2018-01-01', 'Y'), -- People quit
    (102, 'Steven', '2018-01-02', null, 'N'), -- People has start_date update
    (105, 'Lily', '2018-04-01', null, 'N'); -- People newly started

    MERGE INTO employee_trans as tar USING employee_update as src
    ON tar.emp_id = src.emp_id
    WHEN MATCHED and src.quit_flag <> 'Y'
        THEN UPDATE SET start_date src.start_date
    WHEN MATCHED and src.quit_flag = 'Y'
        THEN DELETE
    WHEN NOT MATCHED
        THEN INSERT VALUES (src.emp_id, src.name, src.start_date, src.quit_date, src.quit_flag);

#   SHOWING TRANSACTIONS
    SHOW TRANSACTIONS;

#   Abort transaction
    The ABORT TRANSACTIONS transaction_id statement has been used to kill a transaction with a specified ID 
    since Hive v2.1.0.







