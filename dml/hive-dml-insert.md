
#	Data exchange with INSERT
	
	To extract data from tables/partitions, we can use the INSERT keyword.
	Like other relational databases, Hive supports inserting data into a table by selecting data from another table.

#	Populate data from query while "INTO" will append data
	
	INSERT INTO TABLE employee SELECT * FROM ctas_employee;

#	Verify the data loaded

	SELECT name, work_place FROM employee;

#	Insert a table with specified columns.
	
	 CREATE TABLE emp_simple(
	 name string,
	 work_place string
	 );

#	Specify which columns to insert 
	
	INSERT INTO TABLE emp_simple (name)
	SELECT name FROM employee WHERE name = 'Will';

#	
	INSERT INTO TABLE emp_simple VALUES
	('Michael', 'Toronto'),('Lucy', 'Montreal');

#	Insert data from the CTE statement.

	WITH a as (
	SELECT * FROM ctas_employee
	)
	FROM a
	INSERT OVERWRITE TABLE employee
	SELECT *;

#	Run multi-insert by only scanning the source table once for better performance.
	
	FROM ctas_employee
	INSERT OVERWRITE TABLE employee
	SELECT *
	INSERT OVERWRITE TABLE employee_internal
	SELECT *
	INSERT OVERWRITE TABLE employee_partitioned
	PARTITION (year=2018, month=9)
	SELECT *;
 
