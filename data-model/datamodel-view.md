
#	Views
	Views are logical data structures that can be used to simplify queries by hiding the complexities, such as joins, subqueries, and filters.
	It is called logical because views are only defined in metastore without the footprint in HDFS.
	Unlike what's in the relational database, views in HQL do not store data or get materialized.
	Once the view is created, its schema is frozen immediately.
	Subsequent changes to the underlying tables (for example, adding a column) will not be reflected in the view's schema.
	If an underlying table is dropped or changed, subsequent attempts to query the invalid view will fail.
	In addition, views are read-only and may not be used as the target of the LOAD/INSERT/ALTER statements.


  
