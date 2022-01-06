#   Built-in Table-Generating Functions (UDTF)

    Normal user-defined functions, such as concat(), take in a single input row and output a single output row.
    
    In contrast, table-generating functions transform a single input row to multiple output rows.


#   Explode Array
    explode(ARRAY<T> a)

    Explodes an array to multiple rows.
    Returns a row-set with a single column (col), one row for each element from the array.

    select explode(array('A','B','C'));
    select explode(array('A','B','C')) as col;
    select tf.* from (select 0) t lateral view explode(array('A','B','C')) tf;
    select tf.* from (select 0) t lateral view explode(array('A','B','C')) tf as col;


#   Explod map
    
    explode(MAP<Tkey,Tvalue> m)
    
    Explodes a map to multiple rows. Returns a row-set with a two columns (key,value) , one row for each key-value pair from the input map.

    select explode(map('A',10,'B',20,'C',30));
    
    select explode(map('A',10,'B',20,'C',30)) as (key,value);
    
    select tf.* from (select 0) t lateral view explode(map('A',10,'B',20,'C',30)) tf;
    
    select tf.* from (select 0) t lateral view explode(map('A',10,'B',20,'C',30)) tf as key,value;

#   collect_set and collect_list

    collect_set() and collect_list() do the returning a set/list of elements from each group.
    
    SELECT collect_set(gender_age.gender) as gender_set,
    collect_list(gender_age.gender) as gender_list
    FROM employee;



