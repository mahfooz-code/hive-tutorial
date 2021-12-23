# Hive union
  When we want to combine data with the same schema together, we often use set operations.
  Regular set operations in the relational database are INTERSECT, MINUS, and UNION/UNION ALL.
  HQL only supports UNION and UNION ALL.
  The difference between them is that UNION ALL does not remove duplicate rows while UNION does.
  In addition, all unioned data must have the same name and data type, or else an implicit conversion will be done and may cause a runtime exception.
  If ORDER BY, SORT BY, CLUSTER BY, DISTRIBUTE BY, or LIMIT are used, they are applied to the whole result set after the union:
  
  SELECT a.name as nm FROM employee a
  UNION ALL -- Use column alias to make the same name for union
  SELECT b.name as nm FROM employee_hr b;
