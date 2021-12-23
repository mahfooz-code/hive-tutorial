#	JOIN

	JOIN is used to link rows from two or more tables together.
	Hive supports most SQL JOIN operations, such as INNER JOIN and OUTER JOIN.
	When JOIN is performed between multiple tables, Yarn/MapReduce jobs are created to process the data in the HDFS.
	Each of the jobs is called a stage.
	

#	PREPARING DATA
	
	CREATE TABLE IF NOT EXISTS employee_hr (
	name string,
	employee_id int,
	sin_number string,
	start_date date
	)
	ROW FORMAT DELIMITED
	FIELDS TERMINATED BY '|';
	
	*LOADING DATA*
	
	LOAD DATA LOCAL INPATH '/home/malam/employee_hr.txt'
	OVERWRITE INTO TABLE employee_hr;

#	INNER JOIN
	
	INNER JOIN or JOIN returns rows meeting the join conditions from both sides of joined tables.
	
	It is equivalent to m ∩ n.
	
	SELECT emp.name, emph.sin_number
	FROM employee emp JOIN employee_hr emph ON emp.name = emph.name;
	
	SELECT emp.name, emph.sin_number FROM employee emp JOIN employee_hr emph 
	ON IF(emp.name = 'Will', '1', emp.name) = CASE WHEN emph.name = 'Will' THEN '0' ELSE emph.name END;
	
	
	SELECT emp.name, emph.sin_number
	FROM employee emp
	JOIN employee_hr emph ON emp.name = emph.name
	WHERE emp.name = 'Will';


#	JOIN OPEARATION ON MULTIPLE
	
	The JOIN operation can be performed on more tables (such as table A, B, and C) with sequence joins.
	The tables can either join from A to B and B to C, or join from A to B and A to C.
	
	SELECT emp.name, empi.employee_id, emph.sin_number
	FROM employee emp
	JOIN employee_hr emph ON emp.name = emph.name
	JOIN employee_id empi ON emp.name = empi.name;
	
	
	# The join condition uses different columns, which will create an additional job:
	SELECT emp.name, empi.employee_id, emph.sin_number
	FROM employee emp
	JOIN employee_hr emph ON emp.name = emph.name
	JOIN employee_id empi ON emph.employee_id = empi.employee_id;
	

#	SELF JOIN
	
	Self-join is where one table joins itself.
	When doing such joins, a different alias should be given to distinguish the same table.
	
	
	
#	ENEQUAL JOIN  
	
	It is join supported since v2.2.0  
	
	SELECT emp.name, emph.sin_number FROM employee emp JOIN employee_hr emph ON emp.name != emph.name;
	  

#	IMPLICIT JOIN
	
	The JOIN keyword can also be omitted by comma-separated table names; this is called an implicit join.
	
	SELECT emp.name, emph.sin_number
	FROM employee emp, employee_hr emph -- Only applies for inner join
	WHERE emp.name = emph.name;
	

#	OUTER JOIN
	
	Besides INNER JOIN, HQL also supports regular OUTER JOIN and FULL JOIN. 
	The logic of such a join is the same as what's in the SQL.
	

#	LEFT OUTER JOIN
	
	This returns all rows in the left table and matched rows in the right table.
	If there is no match in the right table, it returns NULL in the right table.
	
	SELECT emp.name, emph.sin_number
	FROM employee emp -- All rows in left table returned
	LEFT JOIN employee_hr emph ON emp.name = emph.name; 

#	RIGHT OUTER JOIN
	
	This returns all rows in the right table and matched rows in the left table.
	If there is no match in the left table, it returns NULL in the left table.
	
	SELECT emp.name, emph.sin_number
	FROM employee emp -- All rows in right table returned
	RIGHT JOIN employee_hr emph ON emp.name = emph.name;

#	FULL OUTER JOIN
	
	This returns all rows in both tables and matched rows in both tables.
	If there is no match in the left or right table, it returns NULL instead.
	It is equal to m + n - m ∩ n.
	
	SELECT emp.name, emph.sin_number
	FROM employee emp -- Rows from both side returned
	FULL JOIN employee_hr emph ON emp.name = emph.name;

#	CROSS JOIN
	
	This returns all row combinations in both the tables to produce a Cartesian product.
	The CROSS JOIN statement does not have a join condition.
	The CROSS JOIN statement can also be written using join without condition or with the always true condition, such as 1 = 1.
	It has m * n rows.
	
	SELECT emp.name, emph.sin_number
	FROM employee emp
	CROSS JOIN employee_hr emph;
	
	
	SELECT emp.name, emph.sin_number
	FROM employee emp
	JOIN employee_hr emph;
	
	SELECT emp.name, emph.sin_number
	FROM employee emp
	JOIN employee_hr emph on 1=1;
	
	

#	MAP JOIN
	
	HQL also supports some special joins that we usually do not see in relational databases, such as MapJoin and Semi-join.
	MapJoin means doing the join operation only with map, without the reduce job.
	The MapJoin statement reads all the data from the small table to memory and broadcasts to all maps.
	During the map phase, the join operation is performed by comparing each row of data in the big table with small tables against the join conditions.
	Because there is no reduce needed, such kinds of join usually have better performance.
	In the newer version of Hive, Hive automatically converts join to MapJoin at runtime if possible. 
	However, you can also manually specify the broadcast table by providing a join hint, /*+ MAPJOIN(table_name) */
	In addition, MapJoin can be used for unequal joins to improve performance since both MapJoin and WHERE are performed in the map phase.
	The following is an example of using a MapJoin hint with CROSS JOIN:
	
	SELECT /*+ MAPJOIN(employee) */ emp.name, emph.sin_number
	FROM employee emp
	CROSS JOIN employee_hr emph 
	WHERE emp.name <> emph.name;
	
	The MapJoin operation does not support the following:

	1) Using MapJoin after UNION ALL, LATERAL VIEW, GROUP BY/JOIN/SORT BY/CLUSTER, and BY/DISTRIBUTE BY
	2) Using MapJoin before UNION, JOIN, and another MapJoin
	
	Bucket MapJoin is a special type of MapJoin that uses bucket columns (the column specified by CLUSTERED BY in the CREATE TABLE statement) as the join condition.
	Instead of fetching the whole table, as done by the regular MapJoin, bucket MapJoin only fetches the required bucket data.
	To enable bucket MapJoin, we need to enable some settings and make sure the bucket number is are multiple of each other. 
	If both joined tables are sorted and bucketed with the same number of buckets, a sort-merge join can be performed instead of caching all small tables in the memory:
	
	SET hive.optimize.bucketmapjoin = true;
	SET hive.optimize.bucketmapjoin.sortedmerge = true;
	SET hive.input.format = org.apache.hadoop.hive.ql.io.BucketizedHiveInputFormat; 

#	SEMI JOIN
	
	In addition, the LEFT SEMI JOIN statement is also a type of MapJoin.
	It is the same as a subquery with IN/EXISTS after v0.13.0 of Hive.
	However, it is not recommended for use since it is not part of standard SQL:

	SELECT a.name FROM employee a
	LEFT SEMI JOIN employee_id b ON a.name = b.name; 

#	BEST PRACTICES
	Usually, it is suggested to put the big table right at the end of the JOIN statement for better 
	performance and to avoid Out Of Memory (OOM) exceptions.
	This is because the last table in the JOIN sequence is usually streamed through reducers where as the 
	others are buffered in the reducer by default.
	
	SELECT /*+ STREAMTABLE(employee_hr) */
	emp.name, empi.employee_id, emph.sin_number
	FROM employee emp
	JOIN employee_hr emph ON emp.name = emph.name
	JOIN employee_id empi ON emph.employee_id = empi.employee_id;
	 
  
