#	Hive union
	When we want to combine data with the same schema together, we often use set operations.
	Regular set operations in the relational database are INTERSECT, MINUS, and UNION/UNION ALL.
	HQL only supports UNION and UNION ALL.
	The difference between them is that UNION ALL does not remove duplicate rows while UNION does.
	In addition, all unioned data must have the same name and data type, or else an implicit conversion will be done and may cause a runtime exception.
	If ORDER BY, SORT BY, CLUSTER BY, DISTRIBUTE BY, or LIMIT are used, they are applied to the whole result set after the union:
  
#	Joining data from two tables
	
	SELECT a.name as nm FROM employee a
	UNION ALL -- Use column alias to make the same name for union
	SELECT b.name as nm FROM employee_hr b;

#	Joining two tables without duplicate data
	SELECT a.name as nm FROM employee a
	UNION -- UNION removes duplicated names and slower
	SELECT b.name as nm FROM employee_hr b;

#	
	Order by applies to the unioned data
	When you want to order only one data set,
	Use order in the subquery
	
	SELECT a.name as nm FROM employee a
	UNION ALL
	SELECT b.name as nm FROM employee_hr b
	ORDER BY nm;


#	Use join for set intercept
	
	SELECT a.name
	FROM employee a
	JOIN employee_hr b ON a.name = b.name;

# Use left join for set minus
	
	SELECT a.name
	FROM employee a
	LEFT JOIN employee_hr b ON a.name = b.name
	WHERE b.name IS NULL;
	
