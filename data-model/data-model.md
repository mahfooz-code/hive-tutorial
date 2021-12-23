# Data type

# INYINT	
It has 1 byte, from -128 to 127. 
The postfix is Y. 
It is used as a small range of numbers.	
10Y

# SMALLINT	
It has 2 bytes, from -32,768 to 32,767. The postfix is S. It is used as a regular descriptive number.	10S

# INT	
It has 4 bytes, from -2,147,483,648 to 2,147,483,647.	10

# BIGINT	
It has 8 bytes, from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807. The postfix is L.	100L

# FLOAT	

This is a 4 byte single-precision floating-point number, from 1.40129846432481707e-45 to 3.40282346638528860e+38 (positive or negative). 

Scientific notation is not yet supported. It stores very close approximations of numeric values.	1.2345679

# DOUBLE	

This is an 8 byte double-precision floating-point number, from 4.94065645841246544e-324d to 1.79769313486231570e+308d (positive or negative).

Scientific notation is not yet supported.

It stores very close approximations of numeric values.	1.2345678901234567

# BINARY	
This was introduced in Hive 0.8.0 and only supports CAST to STRING and vice versa.	1011

# BOOLEAN	

This is a TRUE or FALSE value.	TRUE

# STRING	

This includes characters expressed with either single quotes (') or double quotes ("). 
Hive uses C-style escaping within the strings. 
The max size is around 2 G.	'Books' or "Books"

# CHAR	

The maximum length is fixed at 255.	'US' or "US"

# VARCHAR	

The maximum length is fixed at 65,355.
If a string value being converted/assigned to a varchar value exceeds the length specified, the string is silently truncated.	'Books' or "Books"

# DATE	

This describes a specific year, month, and day in the format of YYYY-MM-DD. 
It is available starting with Hive 0.12.0. 
The range of dates is from 0000-01-01 to 9999-12-31.	2013-01-01

# TIMESTAMP	

This describes a specific year, month, day, hour, minute, second, and millisecond in the format of YYYY-MM-DD HH:MM:SS[.fff...]. 

2013-01-01 12:00:01.345

# Complex type

# ARRAY	

This is a list of items of the same type, such as [val1, val2, and so on]. 

You can access the value using array_name[index], for example, fruit[0]="apple". Index starts from 0.	["apple","orange","mango"]

# MAP	

This is a set of key-value pairs, such as {key1, val1, key2, val2, and so on}. 

You can access the value using map_name[key] for example, fruit[1]="apple".	

{1: "apple",2: "orange"}

# STRUCT	

This is a user-defined structure of any type of field, such as {val1, val2, val3, and so on}. 

By default, STRUCT field names will be col1, col2, and so on. 

You can access the value using structs_name.column_name, for example, fruit.col1=1.	{1, "apple"}

# NAMED STRUCT	

This is a user-defined structure of any number of typed fields, such as {name1, val1, name2, val2, and so on}. 

You can access the value using structs_name.column_name, for example, fruit.apple="gala".	

{"apple":"gala","weight kg":1}

# UNION	

This is a structure that has exactly any one of the specified data types. 

It is available starting with Hive 0.7.0. 

It is not commonly used.	

{2:["apple","orange"]}


#	Login into hive
	
	beeline -u "jdbc:hive2://localhost:10000/default"
	
	CREATE TABLE employee(name STRING,work_place ARRAY<STRING>,gender_age STRUCT<gender:STRING,age:INT>,skills_score MAP<STRING,INT>,depart_title MAP<STRING,ARRAY<STRING>> ) ROW FORMAT DELIMITED FIELDS TERMINATED BY '|'  COLLECTION ITEMS TERMINATED BY ',' MAP KEYS TERMINATED BY ':' STORED AS TEXTFILE

	!table employee
	
	!column employee
	

#	Load data from local file system into the table:

	LOAD DATA LOCAL INPATH '/home/malam/employee.txt' OVERWRITE INTO TABLE employee;


#	Load data from hdfs file system into the table:

	LOAD DATA  INPATH 'employee.txt' OVERWRITE INTO TABLE employee;
