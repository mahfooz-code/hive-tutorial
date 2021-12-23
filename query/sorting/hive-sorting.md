#	Hive Sorting
	Another aspect of manipulating data is properly sorting it in order to clearly identify important facts, such as top the N values, maximum, minimum, and so on.

#	ORDER BY [ASC|DESC]
	It is similar to the SQL ORDER BY statement.
	When using ORDER BY, a sorted order is maintained across all of the output from every reducer.
	It performs a global sort using only one reducer, so it takes longer to return the result.
	The direction specifier after ORDER BY can be either ASC for ascending (low to high) or DESC for descending (high to low).
	If you do not provide a direction specifier, the default of ascending is used. Since v2.1.0, the ORDER BY statement supports 
	specifying the sorting direction for the NULL value, such as NULL FIRST or NULL LAST. 

#	ASC
	By default, NULL stays at the first place in the ASC direction.
	
	SELECT name FROM employee ORDER BY name ASC;
	
#	DESC
	By default, NULL stays at the last place in the DESC direction.
	SELECT name FROM employee ORDER BY name DESC;

#	Order expression
	SELECT name
	FROM employee  -- Order by expression
	ORDER BY CASE WHEN name = 'Will' THEN 0 ELSE 1 END DESC;

#	
	SELECT * FROM emp_simple ORDER BY work_place NULL LAST;

#	SORT BY [ASC|DESC]:
	It specifies which columns to use to sort reducer input records.
	This means the sorting is completed before sending data to the reducer.
	The SORT BY statement does not perform a global sort (but ORDER BY does) and only ensures data is locally sorted in each reducer.
	If SORT BY sorts with only one reducer (set mapred.reduce.tasks=1), it is equal to ORDER BY.
	
	 SET mapred.reduce.tasks = 2;
	 SELECT name FROM employee SORT BY name DESC;
	 
	 Once result is collected to client, it is order-less
	 
	 SET mapred.reduce.tasks = 1;
	 SELECT name FROM employee SORT BY name DESC;

#	DISTRIBUTE BY:
	It is very similar to GROUP BY when the mapper decides to which reducer it can deliver the output.
	Compared to GROUP BY, DISTRIBUTE BY will not work on data aggregations, such as count(*), but only directs where data goes.
	In this case, DISTRIBUTE BY is quite often used to reorganize data in files by specified columns.
	For example, we may need to use DISTRIBUTE BY after a UNION result set to reorganize data in higher granularity.
	When used with SORT BY to sort data within specified groups, DISTRIBUTE BY can be used before SORT BY in one query.
	
	Error when not specify distributed column employee_id in select
	SELECT name FROM employee_hr DISTRIBUTE BY employee_id;
	
	SELECT name, employee_id FROM employee_hr DISTRIBUTE BY employee_id; 

#	CLUSTER BY:
	It is a shortcut operator you can use to perform DISTRIBUTE BY and SORT BY operations on the same group of columns.
	The CLUSTER BY statement does not allow you to specify ASC or DESC yet.
	Compared to ORDER BY, which is globally sorted, the CLUSTER BY statement sorts data in each distributed group:
	
	SELECT name, employee_id FROM employee_hr CLUSTER BY name;
	
	
	
	
	
	
