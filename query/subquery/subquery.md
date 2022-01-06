#	SubQuery
	
	A nested query, which is also called a subquery, is a query projecting data from the result of another query.
	A subquery is a query contained within another SQL statement.
	A subquery is always enclosed within parentheses, and it is usually executed prior to the containing statement. Like any query, a subquery returns a result set that may consist of:

	A single row with a single column
	Multiple rows with a single column
	Multiple rows having multiple columns

	The type of result set returned by the subquery determines how it may be used and which operators the containing statement may use to interact with the data the subquery returns.

	
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

#	Single row returned subquery
	
	SELECT customer_id, first_name, last_name
    	FROM customer
    	WHERE customer_id = (SELECT MAX(customer_id) FROM customer);

	The subquery returns a single row with a single column, which allows it to be used as one of the expressions in an equality condition.


#	Subquery Types

#	Noncorrelated Subqueries

#	Multiple-Row, Single-Column Subqueries

	SELECT country_id
	FROM country
	WHERE country IN ('Canada','Mexico');

	SELECT city_id, city
    FROM city
    WHERE country_id IN
    (SELECT country_id
    	FROM country
    	WHERE country IN ('Canada','Mexico'));
	
	SELECT city_id, city
    	FROM city
    	WHERE country_id NOT IN
    	(SELECT country_id
    		FROM country
    		WHERE country IN ('Canada','Mexico'));
	
	SELECT first_name, last_name
    	FROM customer
    	WHERE customer_id <> ALL
    	(SELECT customer_id
    		FROM payment
    		WHERE amount = 0);
#	any 

	Like the all operator, the any operator allows a value to be compared to the members of a set of values; unlike all, however, a condition using the any operator evaluates to true as soon as a single comparison is favorable.

	SELECT customer_id, sum(amount)
    	FROM payment
    	GROUP BY customer_id
    	HAVING sum(amount) > ANY
    	(SELECT sum(p.amount)
    		FROM payment p
    		INNER JOIN customer c
    		ON p.customer_id = c.customer_id
    		INNER JOIN address a
    		ON c.address_id = a.address_id
    		INNER JOIN city ct
    		ON a.city_id = ct.city_id
    		INNER JOIN country co
    		ON ct.country_id = co.country_id
    	WHERE co.country IN ('Bolivia','Paraguay','Chile')
    	GROUP BY co.country
    );


#	Multicolumn Subqueries

	SELECT actor_id, film_id
    	FROM film_actor
    	WHERE (actor_id, film_id) IN
    	(SELECT a.actor_id, f.film_id
    		FROM actor a
    		CROSS JOIN film f
    	WHERE a.last_name = 'MONROE'
    	AND f.rating = 'PG');

#	Correlated Subqueries

	A correlated subquery, on the other hand, is dependent on its containing statement from which it references one or more columns.

	Unlike a noncorrelated subquery, a correlated subquery is not executed once prior to execution of the containing statement; instead, the correlated subquery is executed once for each candidate row (rows that might be included in the final results).

	For example, the following query uses a correlated subquery to count the number of film rentals for each customer, and the containing query then retrieves those customers who have rented exactly 20 films:

	SELECT c.first_name, c.last_name
    	FROM customer c
    	WHERE 20 =
    	(SELECT count(*) FROM rental r
    		WHERE r.customer_id = c.customer_id);
	
	The reference to c.customer_id at the very end of the subquery is what makes the subquery correlated; the containing query must supply values for c.customer_id for the subquery to execute. 

	In this case, the containing query retrieves all 599 rows from the customer table and executes the subquery once for each customer, passing in the appropriate customer ID for each execution. If the subquery returns the value 20, then the filter condition is met, and the row is added to the result set.


	SELECT c.first_name, c.last_name
    	FROM customer c
    	WHERE
    	(SELECT sum(p.amount) FROM payment p
    		WHERE p.customer_id = c.customer_id)
    		BETWEEN 180 AND 240;
#	The exists Operator

	You use the exists operator when you want to identify that a relationship exists without regard for the quantity.
	
	For example, the following query finds all the customers who rented at least one film prior to May 25, 2005, without regard for how many films were rented.

	 SELECT c.first_name, c.last_name
    	FROM customer c
    	WHERE EXISTS
    	(SELECT 1 FROM rental r
    		WHERE r.customer_id = c.customer_id
    		AND date(r.rental_date) < '2005-05-25');

	Using the exists operator, your subquery can return zero, one, or many rows, and the condition simply checks whether the subquery returned one or more rows.

	SELECT c.first_name, c.last_name
    	FROM customer c
    	WHERE EXISTS
    	(SELECT r.rental_date, r.customer_id, 'ABCD' str, 2 * 3 / 7 nmbr
    	FROM rental r
    	WHERE r.customer_id = c.customer_id
    	 AND date(r.rental_date) < '2005-05-25');

	SELECT a.first_name, a.last_name
    -> FROM actor a
    -> WHERE NOT EXISTS
    ->  (SELECT 1
    ->   FROM film_actor fa
    ->     INNER JOIN film f ON f.film_id = fa.film_id
    ->   WHERE fa.actor_id = a.actor_id
    ->     AND f.rating = 'R');

#	Data Manipulation Using Correlated Subqueries

	UPDATE customer c
	SET c.last_update =
	(SELECT max(r.rental_date) FROM rental r
	WHERE r.customer_id = c.customer_id);
	

	UPDATE customer c
	SET c.last_update =
	(SELECT max(r.rental_date) FROM rental r
	WHERE r.customer_id = c.customer_id)
	WHERE EXISTS
	(SELECT 1 FROM rental r
	WHERE r.customer_id = c.customer_id);

	DELETE FROM customer
	WHERE 365 < ALL
	(SELECT datediff(now(), r.rental_date) days_since_last_rental
	FROM rental r
	WHERE r.customer_id = customer.customer_id);

#	When to Use Subqueries

#	Subqueries as Data Sources

	SELECT c.first_name, c.last_name, 
      	pymnt.num_rentals, pymnt.tot_payments
    	FROM customer c
    		INNER JOIN
    		(SELECT customer_id, 
    			count(*) num_rentals, sum(amount) tot_payments
    		FROM payment
    		GROUP BY customer_id
    	) pymnt
    	ON c.customer_id = pymnt.customer_id;

#	Data fabrication

	Along with using subqueries to summarize existing data, you can use subqueries to generate data that doesn’t exist in any form within your database.

	For example, you may wish to group your customers by the amount of money spent on film rentals, but you want to use group definitions that are not stored in your database.

	SELECT 'Small Fry' name, 0 low_limit, 74.99 high_limit
		UNION ALL
	SELECT 'Average Joes' name, 75 low_limit, 149.99 high_limit
		UNION ALL
	SELECT 'Heavy Hitters' name, 150 low_limit, 9999999.99 high_limit;

#	Task-oriented subqueries

	SELECT c.first_name, c.last_name, ct.city,
    	sum(p.amount) tot_payments, count(*) tot_rentals
    	FROM payment p
    		INNER JOIN customer c
    		ON p.customer_id = c.customer_id
    		INNER JOIN address a
    		ON c.address_id = a.address_id
    		INNER JOIN city ct
    		ON a.city_id = ct.city_id
    	GROUP BY c.first_name, c.last_name, ct.city;


#	Note
	When using not in or <> all to compare a value to a set of values, you must be careful to ensure that the set of values does not contain a null value, because the server equates the value on the lefthand side of the expression to each member of the set, and any attempt to equate a value to null yields unknown.

	SELECT first_name, last_name
		FROM customer
		WHERE customer_id NOT IN (122, 452, NULL);




	 
