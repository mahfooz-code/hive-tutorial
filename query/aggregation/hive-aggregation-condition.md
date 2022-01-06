#   HAVING

    Since v0.7.0, HAVING has been added to support the conditional filtering of aggregation results directly.
    By using HAVING, we can avoid using a subquery after the GROUP BY statement.

    SELECT gender_age.age
    FROM employee
    GROUP BY gender_age.age
    HAVING count(*)=1;

    SELECT gender_age.age,
    count(*) as cnt -- Support use column alias in HAVING, like ORDER BY
    FROM employee
    GROUP BY gender_age.age
    HAVING cnt=1; 

#   Note
    HAVING supports filtering on regular columns too.
    However, it is recommended to use such a filter type after a WHERE clause rather than HAVING for better performance.

    SELECT a.age
    FROM (
        SELECT count(*) as cnt, gender_age.age
        FROM employee GROUP BY gender_age.age
        ) a
    WHERE a.cnt <= 1;

