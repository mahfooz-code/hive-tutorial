#	SubQuery
	A nested query, which is also called a subquery, is a query projecting data from the result of another query. 2
	
#	Nested query as CTE
	Nested queries can be rewritten using CTE with the WITH and AS keywords.
	When using nested queries, an alias should be given for the inner query, or else Hive will report exceptions.

	**A nested query example with the mandatory alias.**
	
	 SELECT name, gender_age.gender as gender
	 FROM ( SELECT * FROM employee WHERE gender_age.gender = 'Male' ) t1;
	 
	 **A nested query can be rewritten with CTE**
	 
	 WITH t1 as (SELECT * FROM employee WHERE gender_age.gender = 'Male')
	 SELECT name, gender_age.gender as gender FROM t1;

#	constant expression
	SELECT concat('1','+','3','=',cast((1 + 3) as string)) as res;


	 
