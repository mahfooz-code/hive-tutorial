#   Partition table design

    Hive partitioning is one of the most effective ways to improve query performance on larger tables.
    A query with partition filtering will only load data from the specified partitions (sub-directories), 
    so it can execute much faster than a normal query that filters by a non-partitioning field. 
    The selection of the partition key is always an important factor for performance.
    It should always be a low-cardinal attribute to avoid so many sub-directories overhead.


#   Partitions by date and time:
    Use date and time, such as year, month, and day (even hours), as partition keys when data is associated with the date/time columns, such as load_date, business_date, run_date, and so on.

#   Partitions by location: 
    Use country, territory, state, and city as partition keys when data is location related.

#   Partitions by business logic: 
    Use department, sales region, applications, customers, and so on as partition keys when data can be separated evenly by business logic.


#   Bucket table design
    A bucket table organizes data into separate files in HDFS.
    Bucketing can speed up data sampling on buckets.
    Bucketing can also improve join performance if the join keys are also bucket columns because bucketing ensures the keys are present in a certain bucket.

    Better-chosen bucket columns make a bucket table join perform better.

    The best practice for choosing bucket columns is to identify the columns that are most likely used in the filter or join condition in terms of the business logic behind the datasets.

#   Index design
    
