
# Bucket
  
  Besides partition, the bucket is another technique to cluster datasets into more manageable parts to optimize query performance.
  Different from a partition, a bucket corresponds to segments of files in HDFS.
  By using buckets, an HQL query can easily and efficiently do sampling, bucket-side joins, and map-side joins.

# Switch to sample database
	create database if not exists sample;

#	Creating normal table

	CREATE TABLE employee_id (
	name STRING,
	employee_id INT,
	work_place ARRAY<STRING>,
	gender_age STRUCT<gender:STRING,age:INT>,
	skills_score MAP<STRING,INT>,
	depart_title MAP<STRING,ARRAY<STRING>>)
	ROW FORMAT DELIMITED
	FIELDS TERMINATED BY '|'
	COLLECTION ITEMS TERMINATED BY ','
	MAP KEYS TERMINATED BY ':';

# Loading data into normal table
	
	LOAD DATA LOCAL INPATH '/home/malam/employee_id.txt' OVERWRITE INTO TABLE employee_id;
	
#	Creating bucket
	
	CREATE TABLE employee_id_buckets (
	name STRING,
	employee_id INT,
	work_place ARRAY<STRING>,
	gender_age STRUCT<gender:string,age:int>,
	skills_score MAP<string,int>,
	depart_title MAP<string,ARRAY<string>>
	)
	CLUSTERED BY (employee_id) INTO 2 BUCKETS
	ROW FORMAT DELIMITED
	FIELDS TERMINATED BY '|'
	COLLECTION ITEMS TERMINATED BY ','
	MAP KEYS TERMINATED BY ':';
	

#	Number of reducer task
	set map.reduce.tasks = 2

#	Enforcing bucket check
	set hive.enforce.bucketing = true;

# Loading data into bucket table

	We cannot use the LOAD DATA statement, because it does not verify the data against the metadata.
	INSERT should be used to populate the bucket table all the time.
	INSERT OVERWRITE TABLE employee_id_buckets SELECT * FROM employee_id;

# Verify the buckets in the HDFS from shell
	hdfs dfs -ls /user/hive/warehouse/sample.db/employee_id_buckets
	
	


