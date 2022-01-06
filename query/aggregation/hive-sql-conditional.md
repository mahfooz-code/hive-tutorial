#   Conditional Logic

    Conditional logic is simply the ability to take one of several paths during program execution. 
    For example, when querying customer information, you might want to include the customer.active column, which stores 1 to indicate active and 0 to indicate inactive.

    SELECT first_name, last_name,
        CASE
            WHEN active = 1 THEN 'ACTIVE'
            ELSE 'INACTIVE'
        END activity_type
    FROM customer;

#   The case Expression

#   COALESCE(column, 0L)
    COALESCE(column, CAST(0 AS BIGINT))
    COALESCE(column, 0L)

#   nvl(T value, T default_value)

#   Searched case Expressions
    CASE
        WHEN C1 THEN E1
        WHEN C2 THEN E2
        ...
        WHEN CN THEN EN
        [ELSE ED]
    END

    
    the symbols C1, C2, ..., CN represent conditions, and the symbols E1, E2, ..., EN represent expressions to be returned by the case expression. If the condition in a when clause evaluates to true, then the case expression returns the corresponding expression. Additionally, the ED symbol represents the default expression, which the case expression returns if none of the conditions C1, C2, ..., CN evaluate to true (the else clause is optional, which is why it is enclosed in square brackets).


    CASE
        WHEN category.name IN ('Children','Family','Sports','Animation')
            THEN 'All Ages'
        WHEN category.name = 'Horror'
            THEN 'Adult'
        WHEN category.name IN ('Music','Games')
            THEN 'Teens'
        ELSE 'Other'
    END

    SELECT c.first_name, c.last_name,
    ->   CASE
    ->     WHEN active = 0 THEN 0
    ->     ELSE
    ->      (SELECT count(*) FROM rental r
    ->       WHERE r.customer_id = c.customer_id)
    ->   END num_rentals
    -> FROM customer c;

#   Simple case Expressions
    
    The simple case expression is quite similar to the searched case expression but is a bit less flexible.
    CASE V0
        WHEN V1 THEN E1
        WHEN V2 THEN E2
        ...
        WHEN VN THEN EN
        [ELSE ED]
    END

    V0 represents a value, and the symbols V1, V2, ..., VN represent values that are to be compared to V0. The symbols E1, E2, ..., EN represent expressions to be returned by the case expression, and ED represents the expression to be returned if none of the values in the set V1, V2, ..., VN matches the V0 value.

    CASE category.name
        WHEN 'Children' THEN 'All Ages'
        WHEN 'Family' THEN 'All Ages'
        WHEN 'Sports' THEN 'All Ages'
        WHEN 'Animation' THEN 'All Ages'
        WHEN 'Horror' THEN 'Adult'
        WHEN 'Music' THEN 'Teens'
        WHEN 'Games' THEN 'Teens'
        ELSE 'Other'
    END

#   Result Set Transformations
    
    SELECT
        SUM(CASE WHEN monthname(rental_date) = 'May' THEN 1
             ELSE 0 END) May_rentals,
        SUM(CASE WHEN monthname(rental_date) = 'June' THEN 1
             ELSE 0 END) June_rentals,
        SUM(CASE WHEN monthname(rental_date) = 'July' THEN 1
             ELSE 0 END) July_rentals
    FROM rental
    WHERE rental_date BETWEEN '2005-05-01' AND '2005-08-01';

#   Checking for Existence

    SELECT a.first_name, a.last_name,
    ->   CASE
    ->     WHEN EXISTS (SELECT 1 FROM film_actor fa
    ->                    INNER JOIN film f ON fa.film_id = f.film_id
    ->                  WHERE fa.actor_id = a.actor_id
    ->                    AND f.rating = 'G') THEN 'Y'
    ->     ELSE 'N'
    ->   END g_actor,
    ->   CASE
    ->     WHEN EXISTS (SELECT 1 FROM film_actor fa
    ->                    INNER JOIN film f ON fa.film_id = f.film_id
    ->                  WHERE fa.actor_id = a.actor_id
    ->                    AND f.rating = 'PG') THEN 'Y'
    ->     ELSE 'N'
    ->   END pg_actor,
    ->   CASE
    ->     WHEN EXISTS (SELECT 1 FROM film_actor fa
    ->                    INNER JOIN film f ON fa.film_id = f.film_id
    ->                  WHERE fa.actor_id = a.actor_id
    ->                    AND f.rating = 'NC-17') THEN 'Y'
    ->     ELSE 'N'
    ->   END nc17_actor
    -> FROM actor a
    -> WHERE a.last_name LIKE 'S%' OR a.first_name LIKE 'S%';


     SELECT f.title,
    ->   CASE (SELECT count(*) FROM inventory i 
    ->         WHERE i.film_id = f.film_id)
    ->     WHEN 0 THEN 'Out Of Stock'
    ->     WHEN 1 THEN 'Scarce'
    ->     WHEN 2 THEN 'Scarce'
    ->     WHEN 3 THEN 'Available'
    ->     WHEN 4 THEN 'Available'
    ->     ELSE 'Common'
    ->   END film_availability
    -> FROM film f


#   Conditional Updates
    UPDATE customer
SET active =
  CASE
    WHEN 90 <= (SELECT datediff(now(), max(rental_date))
                FROM rental r
                WHERE r.customer_id = customer.customer_id)
      THEN 0
    ELSE 1
  END
WHERE active = 1;

#   Handling Null Values
    SELECT c.first_name, c.last_name,
    CASE
        WHEN a.address IS NULL THEN 'Unknown'
        ELSE a.address
    END address,
    CASE
        WHEN ct.city IS NULL THEN 'Unknown'
        ELSE ct.city
    END city,
    CASE
        WHEN cn.country IS NULL THEN 'Unknown'
        ELSE cn.country
    END country
    FROM customer c
    LEFT OUTER JOIN address a
    ON c.address_id = a.address_id
    LEFT OUTER JOIN city ct
    ON a.city_id = ct.city_id
    LEFT OUTER JOIN country cn
    ON ct.country_id = cn.country_id;