#   Data Aggregation

    Data aggregation is the process of gathering and expressing data in a summary to get more information about particular groups based on specific conditions.

#   GROUP BY

    The basic built-in aggregate functions are usually used with the GROUP BY clause.

    If there is no GROUP BY clause specified, it aggregates over the whole row (all columns) by default.

    Besides aggregate functions, all columns selected must also be included in the GROUP BY clause.

#   Aggregation without GROUP BY columns

    SELECT count(*) as rowcnt1,
    count(1) as rowcnt2 -- same to count(*)
    FROM employee;

    Sometimes, the basic aggregate function call returns the result immediately.
    
    The reason is that Hive fetches such aggregation results directly from the statistics collected.
    
    To get the aggregation by actually running a job, you may need to add a limit or where clause in the query.

#   Aggregation with GROUP BY columns:

    SELECT gender_age.gender, count(*) as row_cnt
    FROM employee
    GROUP BY gender_age.gender;

#   Multiple aggregate functions in the same SELECT statement:

    SELECT gender_age.gender, avg(gender_age.age) as avg_age,
    count(*) as row_cnt
    FROM employee GROUP BY gender_age.gender; 

#   Aggregation with CASE WHEN THEN ELSE END

    NULL record will not be counted in count function.

    SELECT sum(
        CASE WHEN gender_age.gender = 'Male'
                THEN gender_age.age
            ELSE 0
        END)/
        count(
            CASE WHEN gender_age.gender = 'Male'
                    THEN 1
                ELSE NULL
            END
        ) as male_age_avg
    FROM employee;

#   Aggregation with coalesce
    
    SELECT sum(coalesce(gender_age.age,0)) as age_sum,
    sum(if(gender_age.gender = 'Female',gender_age.age,0)) as female_age_sum,
    sum(if(gender_age.gender = 'Male',gender_age.age,0)) as male_age_sum
    FROM employee;

#   GROUP BY can also apply to expressions:

    SELECT if(name = 'Will', 1, 0) as name_group,
    count(name) as name_cnt
    FROM employee
    GROUP BY if(name = 'Will', 1, 0);


    ELECT extract(YEAR FROM rental_date) year,
     COUNT(*) how_many
     FROM rental
     GROUP BY extract(YEAR FROM rental_date);


#   Verify that nested aggregate functions are not allowed.

    SELECT avg(count(*)) as row_cnt FROM employee;

#   Aggregate functions such as max or min apply to NULL and return NULL.

    sum and avg cannot apply to NULL.
    The count(null) returns 0.

    SELECT max(null), min(null), count(null);

    Only numeric or string type arguments are accepted but void is passed.
    SELECT sum(null), avg(null);

    CREATE TABLE number_tbl (val SMALLINT);

#   dealing with aggregation across columns with a NULL value.

    CREATE TABLE t (val1 int, val2 int);
    INSERT INTO TABLE t VALUES (1, 2),(null,2),(2,3);
    SELECT * FROM t;

    -- The 2nd row (NULL, 2) is ignored when doing sum(val1 + val2)
    SELECT sum(val1), sum(val1 + val2) FROM t;

    SELECT sum(coalesce(val1,0)),
    sum(coalesce(val1,0) + val2)
    FROM t

#   Aggregate functions With DISTINCT keyword to aggregate on uniquely

    SELECT count(DISTINCT gender_age.gender) as gender_uni_cnt,
    count(DISTINCT name) as name_uni_cnt
    FROM employee;

    When we use COUNT and DISTINCT together, it always ignores the setting (such as mapred.reduce.tasks = 20) for the number of reducers used and may use only one reducer.

    In this case, the single reducer becomes the bottleneck when processing large volumes of data.

    SELECT count(distinct gender_age.gender) as gender_uni_cnt FROM employee;

#   Use subquery to select unique value before aggregations
    
    SELECT count(*) as gender_uni_cnt
    FROM (
        SELECT DISTINCT gender_age.gender FROM employee
    ) a;

#   who are the oldest males and females with ages in the employee table
    SELECT gender_age.gender,
    max(struct(gender_age.age, name)).col1 as age,
    max(struct(gender_age.age, name)).col2 as name
    FROM employee
    GROUP BY gender_age.gender;

#   Group filter

    SELECT customer_id, count(*)
        FROM rental
        GROUP BY customer_id
        HAVING count(*) >= 40;

#   Generating Groups

#   Single-Column Grouping

    SELECT actor_id, count(*)
     FROM film_actor
     GROUP BY actor_id;

#   Multicolumn Grouping

    SELECT fa.actor_id, f.rating, count(*)
     FROM film_actor fa
     INNER JOIN film f
     ON fa.film_id = f.film_id
     GROUP BY fa.actor_id, f.rating
     ORDER BY 1,2;

#   Note
    
    The hive.map.aggr property controls aggregations in the map task.
    The default value for this setting is true, so Hive will do the first-level aggregation directly in the map task for better performance, but consume more memory.
    Turn it off if you run out of memory in the map phase.

