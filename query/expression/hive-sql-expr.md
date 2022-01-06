#   Common table expressions

    Common table expressions (a.k.a. CTEs), which are new to MySQL in version 8.0, have been available in other database servers for quite some time.
    
    A CTE is a named subquery that appears at the top of a query in a with clause, which can contain multiple CTEs separated by commas.
    
    Along with making queries more understandable, this feature also allows each CTE to refer to any other CTE defined above it in the same with clause.
    
    The following example includes three CTEs, where the second refers to the first, and the third refers to the second.

    WITH    actors_s AS
            (SELECT actor_id, first_name, last_name
                FROM actor
                WHERE last_name LIKE 'S%'
            ),
            actors_s_pg AS
            (SELECT s.actor_id, s.first_name, s.last_name,
                f.film_id, f.title
            FROM actors_s s
            INNER JOIN film_actor fa
            ON s.actor_id = fa.actor_id
            INNER JOIN film f
            ON f.film_id = fa.film_id
            WHERE f.rating = 'PG'
            ),
            actors_s_pg_revenue AS
            (SELECT spg.first_name, spg.last_name, p.amount
            FROM actors_s_pg spg
            INNER JOIN inventory i
            ON i.film_id = spg.film_id
            INNER JOIN rental r
            ON i.inventory_id = r.inventory_id
            INNER JOIN payment p
            ON r.rental_id = p.rental_id
            ) -- end of With clause
    SELECT spg_rev.first_name, spg_rev.last_name, sum(spg_rev.amount) tot_revenue
    FROM actors_s_pg_revenue spg_rev
    GROUP BY spg_rev.first_name, spg_rev.last_name
    ORDER BY 3 desc;

#   Subqueries as Expression Generators
    SELECT
    (SELECT c.first_name FROM customer c
        WHERE c.customer_id = p.customer_id
    ) first_name,
    (SELECT c.last_name FROM customer c
    WHERE c.customer_id = p.customer_id
    ) last_name,
    (SELECT ct.city
    FROM customer c
    INNER JOIN address a
    ON c.address_id = a.address_id
    INNER JOIN city ct
    ON a.city_id = ct.city_id
    WHERE c.customer_id = p.customer_id
    ) city,
    sum(p.amount) tot_payments,
    count(*) tot_rentals
    FROM payment p
    GROUP BY p.customer_id;

    SELECT a.actor_id, a.first_name, a.last_name
    -> FROM actor a
    -> ORDER BY
    ->  (SELECT count(*) FROM film_actor fa
    ->   WHERE fa.actor_id = a.actor_id) DESC;
    