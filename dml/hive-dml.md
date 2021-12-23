#	LOAD

#	LOADING FROM LOCAL HOST

	The LOCAL keyword in the LOAD statement specifies where the files are located on the client host.
	If the LOCAL keyword is not specified, the files are loaded from the full Uniform Resource Identifier (URI) specified after INPATH (hdfs path) or the value from
	the fs.default.name property defined in hdfs-site.xml by default.
	The path after INPATH can be a relative path or an absolute path.

#	Load local data in a table, internal or external.
	
	LOAD DATA LOCAL INPATH '/home/malam/employee_hr.txt'
	OVERWRITE INTO TABLE employee_hr;

#	Load the local data to a partition.

	LOAD DATA LOCAL INPATH '/home/malam/employee_hr.txt'
	OVERWRITE INTO TABLE employee_partitioned
	PARTITION (year=2018, month=12);

#	Load data from HDFS to a table using the URI.
	
	LOAD DATA INPATH '/home/malam/employee_hr.txt' INTO TABLE employee;

#	Use full URI
	
	LOAD DATA INPATH
	'hdfs://localhost:9000/tmp/hivedemo/data/employee.txt'


  
	
