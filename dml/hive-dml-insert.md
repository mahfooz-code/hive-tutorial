
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
 
#	Inserting data into dynamic partition table.

	SET hive.exec.dynamic.partition=true;
	
	By default, the user must specify at least one static partition column.
	This is to avoid accidentally overwriting partitions.
	To disable this restriction, we can set the partition mode to nonstrict from the default strict 
	mode before inserting into dynamic partitions as follows:
	
	SET hive.exec.dynamic.partition.mode=nonstrict;

#	Partition year, month are determined from data

	INSERT INTO TABLE employee_partitioned PARTITION(year, month)
	SELECT name, array('Toronto') as work_place,
	named_struct("gender","Male","age",30) as gender_age,
	map("Python",90) as skills_score,
	map("R&D",array('Developer')) as depart_title,
	'C3A',
	year(start_date) as year, month(start_date) as month
	FROM employee_hr eh
	WHERE eh.employee_id = 102;

#	Downloading into local directory and by default it downloads in hdfs.

	INSERT OVERWRITE LOCAL DIRECTORY 'file:/home/malam/hive_download'
	SELECT * FROM employee;
	
	
#	 
	INSERT OVERWRITE LOCAL DIRECTORY '/tmp/output2'
	ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
	SELECT * FROM employee;

#	Use multi-insert statements to export data from the same table:
	
	FROM employee
	INSERT OVERWRITE DIRECTORY '/user/malam/output3'
	SELECT *
	INSERT OVERWRITE DIRECTORY '/user/malam/output4'
	SELECT name ;
      
     
  
#	Append to local files:
	
	hive -e 'select * from employee' >> test

#	Overwrite local files:
	
	hive -e 'select * from employee' > test

#	Append to HDFS files:
	hive -e 'select * from employee'|hdfs dfs -appendToFile - /tmp/test1 

#	Overwrite HDFS files: 
	
	hive -e 'select * from employee'|hdfs dfs -put -f - /tmp/test2

	
