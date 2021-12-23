
#	Create the database/schema if it doesn't exist:
	
	CREATE DATABASE myhivebook
	CREATE SCHEMA IF NOT EXISTS myhivebook; 

If the database is not specified, the default database is used and uses /user/hive/warehouse in HDFS as its root directory. 

This path is configurable by the hive.metastore.warehouse.dir property in hive-site.xml.

Whenever a new database is created, Hive creates a new directory for each database under /user/hive/warehouse

For example, the myhivebook database is located at /user/hive/datawarehouse/myhivebook.db.


#	Create the database with the location, comments, and metadata information:
	
	CREATE DATABASE IF NOT EXISTS myhivebook
	COMMENT 'hive database demo'
	LOCATION '/user/malam/hive/myhivebook'
	WITH DBPROPERTIES ('creator'='malam','date'='2018-05-01');

#	To show the DDL use show create database SHOW DATABASES;

	SHOW CREATE DATABASE default;

#	Show and describe the database with wildcards:
	SHOW DATABASES;
	SHOW DATABASES LIKE 'my.*';
	DESCRIBE DATABASE default;

#	Switch to use one database or directly qualify the table name with the database name.
	USE myhivebook;

#	Show the current database:
	SELECT current_database();

#	Drop the database:
	DROP DATABASE IF EXISTS myhivebook;--failed when database is not empty
	DROP DATABASE IF EXISTS myhivebook CASCADE;--drop database and tables

#	Alter the database properties. 
	The ALTER DATABASE statement can only apply to dbproperties, owner, and location on the database. 
	The other database properties cannot be changed:
	
	ALTER DATABASE myhivebook SET DBPROPERTIES ('edited-by'='malam');
	ALTER DATABASE myhivebook SET OWNER user malam;
	ALTER DATABASE myhivebook SET LOCATION '/tmp/data/myhivebook';
			
