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
	
	
	
