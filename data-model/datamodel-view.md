
#	Views
	Views are logical data structures that can be used to simplify queries by hiding the complexities, such as joins, subqueries, and filters.
	It is called logical because views are only defined in metastore without the footprint in HDFS.
	Unlike what's in the relational database, views in HQL do not store data or get materialized.
	Once the view is created, its schema is frozen immediately.
	Subsequent changes to the underlying tables (for example, adding a column) will not be reflected in the view's schema.
	If an underlying table is dropped or changed, subsequent attempts to query the invalid view will fail.
	In addition, views are read-only and may not be used as the target of the LOAD/INSERT/ALTER statements.


  #	Creating views
  	CREATE VIEW IF NOT EXISTS employee_skills
	AS
	SELECT
	name, skills_score['DB'] as DB,
	skills_score['Perl'] as Perl,
	skills_score['Python'] as Python,
	skills_score['Sales'] as Sales,
	skills_score['HR'] as HR
	FROM employee;

	When creating views, there is no yarn job triggered since this is only a metadata change.

#	Query the view
	
	The job will be triggered when querying the view.

#	Listing all views and it is not working and needs to verify.
	SHOW VIEWS;
	SHOW VIEWS 'employee_*';
	
	Show tables like 'employee_*';

#	Show the defintion of view
	show create table employee_skills; -- this is recommded
	
	 DESC FORMATTED employee_skills;

#	Alter the views' properties:
	ALTER VIEW employee_skills SET TBLPROPERTIES ('comment'='A view');

#	Redefine the views:
	ALTER VIEW employee_skills as SELECT * from employee;

#	Drop the views:
	DROP VIEW employee_skills;

#	Lateral View
	It is usually used with user-defined table-generating functions in Hive, such as explode(), for data normalization or processing JSON data.
	LateralView first applies the table-generation function to the data, and then joins the function's input and output together.
	
