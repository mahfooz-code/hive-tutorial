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
	
	
