#	Table
	The concept of a table in Hive is very similar to the table in the relational database. 
	Each table maps to a directory, which is under /user/hive/warehouse by default in HDFS. 
	For example, /user/hive/warehouse/employee is created for the employee table.

# Internal/Managed Table
	All the data in the table is stored in this hive user-manageable directory (full permission).
	This kind of table is called an internal, or managed, table. 
	When keeping data in the internal tables, the table fully manages the data in it.
	When an internal table is dropped, its data is deleted together.
	On the other hand, the internal table is often used as an intermediate table during data processing, since it is quite powerful and flexible when supported by HQL.

#	Creating internal table
	CREATE TABLE IF NOT EXISTS employee_internal(name STRING COMMENT 'this is optinal column comments',
	work_place ARRAY<STRING>,
	gender_age STRUCT<gender:STRING,age:INT>,
	skills_score MAP<STRING,INT>,
	depart_title MAP<STRING,ARRAY<STRING>>
	)
	COMMENT 'This is an internal table'-- This is optional table comments
	ROW FORMAT DELIMITED
	FIELDS TERMINATED BY '|'     -- Symbol to seperate columns
	COLLECTION ITEMS TERMINATED BY ','-- Seperate collection elements
	MAP KEYS TERMINATED BY ':'  -- Symbol to seperate keys and values
	STORED as TEXTFILE;         -- Table file format
	
# Load data into table
	 LOAD DATA LOCAL INPATH '/home/malam/employee.txt' OVERWRITE INTO TABLE employee_internal;
	

# External Table
	When data is already stored in HDFS, an external table can be created to describe the data. 
	It is called external because the data in the external table is specified in the LOCATION property rather than the default warehouse directory.
	when an external table is dropped, the data is not deleted.
	It is quite common to use external tables for source read-only data or sharing the processed data to data consumers giving customized HDFS locations.
	
# Create an external table and load the data:
	CREATE EXTERNAL TABLE employee_external ( -- Use EXTERNAL keywords
	name string,
	work_place ARRAY<string>,
	gender_age STRUCT<gender:string,age:int>,
	skills_score MAP<string,int>,
	depart_title MAP<STRING,ARRAY<STRING>>)
	ROW FORMAT DELIMITED
	FIELDS TERMINATED BY '|'
	COLLECTION ITEMS TERMINATED BY ','
	MAP KEYS TERMINATED BY ':'
	STORED as TEXTFILE
	LOCATION '/user/malam/hive/employee'; 
	
# load the data using load
	LOAD DATA LOCAL INPATH '/home/malam/employee.txt'
	OVERWRITE INTO TABLE employee_external;

# Temporary table
	Hive also supports creating temporary tables. 
	A temporary table is only visible to the current user session.
	It's automatically deleted at the end of the session. 
	The data of the temporary table is stored in the user's scratch directory, such as /tmp/hive-<username>. 
	Therefore, make sure the folder is properly configured or secured when you have sensitive data in temporary tables. 
	Whenever a temporary table has the same name as a permanent table, the temporary table will be chosen rather than the permanent table.
	A temporary table does not support partitions and indexes. 

#	The following are three ways to create temporary tables:
	CREATE TEMPORARY TABLE IF NOT EXISTS tmp_emp1 (name string,
	work_place ARRAY<string>,
	gender_age STRUCT<gender:string,age:int>,
	skills_score MAP<string,int>,
	depart_title MAP<STRING,ARRAY<STRING>>
	);
	
	CREATE TEMPORARY TABLE tmp_emp2 as SELECT * FROM tmp_emp1;
	CREATE TEMPORARY TABLE tmp_emp3 like tmp_emp1;
	
#	Create-Table-As-Select (CTAS)
	The table created by CTAS is not visible by other users until all the query results are populated. CTAS has the following restrictions:
	
	-The table created cannot be a partitioned table
	-The table created cannot be an external table
	-The table created cannot be a list-bucketing table

# Create a table with CTAS:
	
	CREATE TABLE ctas_employee as SELECT * FROM employee_external;

#	Use CTAS to create an empty table by copying the schema from another table.
	CREATE TABLE empty_ctas_employee as SELECT * FROM employee_internal WHERE 1=2;

# Create a table with both CTAS and CTE
	
	CREATE TABLE cte_employee as
	WITH r1 as (SELECT name FROM r2 WHERE name = 'Michael'),
	r2 as (SELECT name FROM employee WHERE gender_age.gender= 'Male'),
	r3 as (SELECT name FROM employee WHERE gender_age.gender= 'Female')
	SELECT * FROM r1 UNION ALL SELECT * FROM r3;


# CTAS can also be used with CTE, which stands for Common Table Expression.
	CTE is a temporary result set derived from a simple select query specified in a WITH clause, followed by the SELECT or INSERT statement to build the result set.
	The CTE is defined only within the execution scope of a single statement.
	One or more CTEs can be used in a nested or chained way with keywords, such as the SELECT, INSERT, CREATE TABLE AS SELECT, or CREATE VIEW AS SELECT statements.
	Using CTE of HQL makes the query more concise and clear than writing complex nested queries.

#	In another way, we can also use CREATE TABLE LIKE to create an empty table
	 CREATE TABLE empty_like_employee LIKE employee_internal;	
	 CREATE TABLE empty_like_employee LIKE employee_internal;

#	Table description

#	Show tables with regular expression filters:
	SHOW TABLES; 
	SHOW TABLES '*sam*';
	SHOW TABLES '*sam|lily*'; 

#	List detailed table information for all tables matching the given regular expression:
	SHOW TABLE EXTENDED LIKE 'employee_int*';DROP TABLE IF EXISTS empty_ctas_employee;

#	Show table-column information in two ways:
	SHOW COLUMNS IN employee_internal;

	DESC employee_internal;
	
#	Show create-table DDL statements for the specified table:
	SHOW CREATE TABLE employee_internal;
	
#	Show table properties for the specified table:
	SHOW TBLPROPERTIES employee_internal;
	
#	Table cleaning
	DROP TABLE IF EXISTS empty_ctas_employee;

#	The truncate table statement only removes data from the internal table only.
	The table still exists, but is empty. 
	
	 TRUNCATE TABLE cte_employee;

#	Table alteration
	Rename a table with the ALTER statement. 
	This is quite often used as data backup:
	
	ALTER TABLE cte_employee RENAME TO cte_employee_backup;

#	Change the table properties with TBLPROPERTIES:
      	ALTER TABLE cte_employee_backup SET TBLPROPERTIES ('comment'='New comments');

#	Change the table's row format and SerDe with SERDEPROPERTIES:
	ALTER TABLE employee_internal SET SERDEPROPERTIES ('field.delim' = '$');

#	Change the table's file format with FILEFORMAT:
	ALTER TABLE cte_employee_backup SET FILEFORMAT RCFILE;

#	Change the table's location, a full URI of HDFS, with LOCATION:
	ALTER TABLE cte_employee_backup SET LOCATION 'hdfs://localhost:8020/tmp/employee';

#	Enable/Disable the table's protection. NO_DROP or OFFLINE. 
	
#	NO_DROP prevents a table from being dropped
	
	ALTER TABLE cte_employee_backup ENABLE NO_DROP; 
	
#	OFFLINE prevents data (not metadata) from being queried in a table: is not working and needs to check.
	ALTER TABLE cte_employee_backup DISABLE NO_DROP; 
	ALTER TABLE c_employee ENABLE OFFLINE;
	ALTER TABLE c_employee DISABLE OFFLINE;

#	Enable concatenation in an RCFile, or ORC table if it has many small files:
	ALTER TABLE cte_employee_backup SET FILEFORMAT ORC;
	ALTER TABLE cte_employee_backup CONCATENATE;

#	Change the column's data type, position (with AFTER or FIRST), and comment:
	DESC employee_internal;
	
	ALTER TABLE employee_internal CHANGE name employee_name string AFTER gender_age;
	DESC employee_internal; 
	ALTER TABLE employee_internal CHANGE employee_name name string COMMENT 'updated' FIRST;

#	Add new columns to a table:
	ALTER TABLE cte_employee_backup ADD COLUMNS (work string);

#	Replacing all columns
	ALTER TABLE cte_employee_backup REPLACE COLUMNS (name string);

#	Partition
	By default, a simple HQL query scans the whole table.
	In Hive, each partition corresponds to a predefined partition column(s), which maps to subdirectories in the table's directory in HDFS.
	When the table gets queried, only the required partitions (directory) of data in the table are being read, so the I/O and time of the query is greatly reduced.
	Using partition is a very easy and effective way to improve performance in Hive.
	
	CREATE TABLE employee_partitioned (name STRING,
	work_place ARRAY<STRING>,
	gender_age STRUCT<gender:STRING,age:INT>,
	skills_score MAP<STRING,INT>,
	depart_title MAP<STRING,ARRAY<STRING>>
	)
	PARTITIONED BY (year INT, month INT)
	ROW FORMAT DELIMITED
	FIELDS TERMINATED BY '|'
	COLLECTION ITEMS TERMINATED BY ','
	MAP KEYS TERMINATED BY ':'; 

#	Show parititons on a table 
	show partitions employee_partitioned;

#	STATIC PARTITION
	
	ALTER TABLE ADD PARTITION statement to add static partitions to a table.
	This command changes the table's metadata but does not load data.
	If the data does not exist in the partition's location, queries will not return any results.
	
	ALTER TABLE employee_partitioned ADD
	PARTITION (year=2018, month=11)
	PARTITION (year=2018, month=12);
	
#	Load data into a table partition once the partition is created:
	
	LOAD DATA INPATH '/tmp/hivedemo/data/employee.txt'
	OVERWRITE INTO TABLE employee_partitioned
	PARTITION (year=2018, month=12);
	
	SELECT name, year, month FROM employee_partitioned;
	
#	DYNAMIC PARTITION
	

#	To drop the partition metadata, use the ALTER TABLE DROP PARTITION statement. 
	For external tables, ALTER does not change data but metadata, drop partition will not drop data inside the partition. 
	In order to remove data, we can use the hdfs dfs -rm command to remove data from HDFS for the external table.
	For internal tables, ALTER TABLE ... DROP PARTITION will remove both partition and data. 
	
	ALTER TABLE employee_partitioned DROP IF EXISTS PARTITION (year=2018, month=11);
	SHOW PARTITIONS employee_partitioned;
	ALTER TABLE employee_partitioned DROP IF EXISTS PARTITION (year=2017);
	ALTER TABLE employee_partitioned DROP IF EXISTS PARTITION (month=9);
	ALTER TABLE employee_partitioned PARTITION (year=2018, month=12) RENAME TO PARTITION (year=2018,month=10);
	SHOW PARTITIONS employee_partitioned;
	
	Because all partition columns should be specified for partition rename
	
	ALTER TABLE employee_partitioned PARTITION (year=2018) RENAME TO PARTITION (year=2017);
	
	For internal table, we use truncate
	
      	TRUNCATE TABLE employee_partitioned PARTITION (year=2018,month=12);
	
	For external table, we have to use hdfs command
	dfs -rm -r -f /user/malam/employee_partitioned;

#	Add regular columns to a partition table.
	
	Note, we CANNOT add new columns as partition columns.
	There are two options when adding/removing columns from a partition table, CASCADE and RESTRICT.
	The commonly used CASCADE option cascades the same change to all the partitions in the table.
	However,  RESTRICT is the default, limiting column changes only to table metadata, which means the changes will be only applied to new partitions rather than existing 
	partitions:
	
	ALTER TABLE employee_partitioned ADD COLUMNS (work string) CASCADE;
	
	ALTER TABLE employee_partitioned PARTITION COLUMN(year string);
	
	DESC employee_partitioned;

#	Changing the partition's other properties in terms of file format, location, protections, and concatenation have the same syntax to alter the table statement:
	
	ALTER TABLE employee_partitioned PARTITION (year=2018) SET FILEFORMAT ORC;
	
	ALTER TABLE employee_partitioned PARTITION (year=2018) SET LOCATION '/tmp/data';
	
	ALTER TABLE employee_partitioned PARTITION (year=2018) ENABLE NO_DROP;
	
	ALTER TABLE employee_partitioned PARTITION (year=2018) ENABLE OFFLINE;
	
	ALTER TABLE employee_partitioned PARTITION (year=2018) DISABLE NO_DROP;
	
	ALTER TABLE employee_partitioned PARTITION (year=2018) DISABLE OFFLINE;
	
	ALTER TABLE employee_partitioned PARTITION (year=2018) CONCATENATE;
	
	
	
