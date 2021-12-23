#	Filtering data with conditions
	Narrow down the result set by using a condition clause, such as LIMIT, WHERE, IN/NOT IN, and EXISTS/NOT EXISTS.

#	Filtering with LIMIT
	SELECT name FROM employee LIMIT 2;

#	Filtering with WHERE
	SELECT name, work_place FROM employee WHERE name = 'Michael';

#	Filtering with LIMIT AND WHERE
	SELECT name, work_place FROM employee WHERE name = 'Michael' LIMIT 1; 

#	Filtering with IN/NOT IN
	IN/NOT IN is used as an expression to check whether values belong to a set specified by IN or NOT IN.
	
	SELECT name FROM employee WHERE gender_age.age in (27, 30);
	
	**With multiple columns support after v2.1.0**
	
	SELECT name, gender_age
	FROM employee 
	WHERE (gender_age.gender, gender_age.age) IN (('Female', 27), ('Male', 27 + 3));
	
	**filtering data can also use a subquery**
	
	SELECT name, gender_age.gender as gender
	FROM employee
	WHERE name IN
	(SELECT name FROM employee WHERE gender_age.gender = 'Male');

#	Filtering with EXISTS/NOT EXISTS
	
	SELECT
	name, gender_age.gender as gender
	FROM employee a
	WHERE EXISTS (SELECT * FROM employee b WHERE
	a.gender_age.gender = b.gender_age.gender AND
	b.gender_age.gender = 'Male');

#	Restriction on hive where clause
	
	There are additional restrictions for subqueries used in WHERE clauses:

	1) Subqueries can only appear on the right-hand side of WHERE clauses
	2) Nested subqueries are not allowed
	3) IN/NOT IN in subqueries only support the use of a single column, although they support more in regular expressions


	
