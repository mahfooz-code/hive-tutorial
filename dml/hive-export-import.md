#	Hive Export Import
	When working on data migration or release deployment, we may need to move data between different environments or clusters.
	In HQL, EXPORT and IMPORT statements are available to move data between HDFS in different environments or clusters.
	The EXPORT statement exports both data and metadata from a table or partition.
	Metadata is exported in a file called _metadata. 

#	Export table
	
	 EXPORT TABLE employee TO '/tmp/output5';
	 dfs -ls -R /tmp/output5/;


	 


