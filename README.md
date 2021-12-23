# hive-tutorial
**What applications are supported by Hive?**

Hive supports client applications based on Java, PHP, Python, C, and Ruby coding languages.

**WHAT ARE THE DIFFERENT TABLES AVAILABLE IN HIVE?**

There are two types of tables available in Hive – managed and external.

**WHAT IS THE DIFFERENCE BETWEEN EXTERNAL AND MANAGED TABLES?**

While external tables give data control to Hive but not control of a schema, managed tables give both schema and data control.

**WHERE DOES THE DATA OF A HIVE TABLE GET STORED?**

The Hive table gets stored in an HDFS directory – /user/hive/warehouse, by default.

You can adjust it by setting the desired directory in the configuration parameter hive.metastore.warehouse.dir in hive-site.xml.

**CAN HIVE BE USED IN OLTP SYSTEMS?**

Since Hive does not support row-level data insertion, it is not suitable for use in OLTP systems.

**CAN A TABLE NAME BE CHANGED IN HIVE?**

Yes, you can change a table name in Hive.

You can rename a table name by using: 

_Alter Table table_name RENAME TO new_name._

**CAN THE DEFAULT LOCATION OF A MANAGED TABLE BE CHANGED IN HIVE?**

Yes, the default managed table location can be changed in Hive by using the LOCATION ‘<hdfs_path>’ clause.
