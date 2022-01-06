#   Hive Index
    Using indexes is a very common best practice for performance tuning in relational databases.
    
    Hive has supported index creation on tables/partitions since Hive v0.7.0.
    An index in Hive provides a key-based data view and better data access for certain operations, such as WHERE, GROUP BY, and JOIN.
    
    Using an index is always a cheaper alternative than full-table scans.


#   Creating index
    CREATE INDEX idx_id_employee_id
    ON TABLE employee_id (employee_id)
    AS 'COMPACT'
    WITH DEFERRED REBUILD;

#   COMPACT index
    
    It stores the pair of the indexed column's value and its block ID, HQL has also supported BITMAP indexes since v0.8.0 for column values with less variance.

#   Bitmap index
    CREATE INDEX idx_gender_employee_id
    ON TABLE employee_id (gender_age)
    AS 'BITMAP'
    WITH DEFERRED REBUILD;

#   DEFFERED REBUILD
    The WITH DEFERRED REBUILD option prevents the index from immediately being built.

#   Rebuilding index

    When data in the base table changes, the same command must be used again to bring the index up to date.
    This is an atomic operation. 
    If the index rebuilt on a table has been previously indexed failed, the state of the index remains the same.
    
    ALTER INDEX idx_id_employee_id ON employee_id REBUILD;
    ALTER INDEX idx_gender_employee_id ON employee_id REBUILD;

#   Index name format
    Once the index is built, a new index table is created for each index with the name in the format of 
    
    <database_name>__<table_name>_<index_name>__:

    The index table contains the indexed column, the _bucketname (a typical file URI on HDFS), and _offsets (offsets for each row).

#   Show index
    SHOW TABLES '*idx*';

#   Get information about index

    desc sample__employee_id_idx_id_employee_id__;
    SELECT * FROM sample__employee_id_idx_id_employee_id__;

#   Drop index
    To drop an index, we can only use the DROP INDEX index_name ON table_name statement as follows.
    We cannot drop the index with a DROP TABLE statement:

    DROP INDEX idx_gender_employee_id ON employee_id;










