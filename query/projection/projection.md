#	Simple projection
	Most simple SELECT statements will not trigger a Yarn job. 
	Instead, a dump task is created just for dumping the data, such as the hdfs dfs -cat command.
	A FROM keyword followed by a table is where SELECT projects data.

#	Query the whole row or specific columns in the table:
	
	 SELECT * FROM employee;
	 SELECT name FROM employee;

#	Distinct data
	
	The SELECT statement is quite often used with the FROM and DISTINCT keywords. 
	The DISTINCT keyword used after SELECT ensures only unique rows or combination of columns are returned from the table.
	
	SELECT DISTINCT name, work_place FROM employee;

#	 
	In addition, SELECT also supports columns combined with user-defined functions, IF(), or a CASE WHEN THEN ELSE END statement, and regular expressions.

#	List all columns match java regular expression
	SET hive.support.quoted.identifiers = none
	SELECT `^work.*` FROM employee;

#	Conditional selection of data
	SELECT CASE WHEN gender_age.gender = 'Female' THEN 'Ms.' ELSE 'Mr.' END as title,
	name,
	IF(array_contains(work_place,'New York'),'US','CA') as country
	FROM employee

#	subquery
	A nested query, which is also called a subquery, is a query projecting data from the result of another query.
	

	

	
