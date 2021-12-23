#	Hive Export Import
	When working on data migration or release deployment, we may need to move data between different environments or clusters.
	In HQL, EXPORT and IMPORT statements are available to move data between HDFS in different environments or clusters.
	The EXPORT statement exports both data and metadata from a table or partition.
	Metadata is exported in a file called _metadata. 

#	Export table
	
	 EXPORT TABLE employee TO '/tmp/output5';
	 dfs -ls -R /tmp/output5/;
#	Import
	IMPORT TABLE FROM '/tmp/output5';
	IMPORT TABLE empolyee_imported -- Specify a table imported
	FROM '/tmp/output5';

#	Import data to an external table, where the LOCATION property is optional:
	
	IMPORT EXTERNAL TABLE empolyee_imported_external
	FROM '/tmp/output5'
	LOCATION '/tmp/output6';

#	Export and import partitions:
	EXPORT TABLE employee_partitioned partition
	(year=2018, month=12) TO '/tmp/output7';
      
	 


