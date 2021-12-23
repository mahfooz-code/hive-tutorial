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
	It has m * n rows.
	
	SELECT emp.name, emph.sin_number
	FROM employee emp
	CROSS JOIN employee_hr emph;
	
	

#	MAP JOIN

#	SEMI JOIN


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
	 
  
