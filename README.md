# hive-tutorial

Hive provides a simple and optimized query model with less coding than MapReduce

HQL and SQL have a similar syntax

Hive's query response time is typically much faster than others on the same volume of big datasets

Hive supports running on different computing frameworks

Hive supports ad hoc querying data on HDFS and HBase

Hive supports user-defined java/scala functions, scripts, and procedure languages to extend its functionality

Matured JDBC and ODBC drivers allow many applications to pull Hive data for seamless reporting

Hive allows users to read data in arbitrary formats, using SerDes and Input/Output formats

Hive is a stable and reliable batch-processing tool, which is production-ready for a long time

Hive has a well-defined architecture for metadata management, authentication, and query optimizations

There is a big community of practitioners and developers working on and using Hive


# ****Client****

**What applications are supported by Hive?**

Hive supports client applications based on Java, PHP, Python, C, and Ruby coding languages.



# ****Hive cli****

# ****Hive Web Interface****

# ****Thrift server****

# ****Hive jdbc****

# ****Modes****

**WHAT ARE THE THREE DIFFERENT MODES IN WHICH HIVE CAN BE OPERATED?**

The three modes in which Hive can be operated are **Local mode, distributed mode, and pseudo-distributed mode.**


# ****Data model****

**WHAT ARE THE DIFFERENT TABLES AVAILABLE IN HIVE?**

There are two types of tables available in Hive – managed and external.

# ****Driver****

# ****Metastore****

**WHAT IS A HIVE METASTORE?**

A Metastore is a relational database that stores the metadata of Hive partitions, tables, databases, and so on.

**WHAT ARE THE TYPES OF META STORES?**

Local and Remote meta stores are the two types of Hive meta stores.

**WHAT IS THE DIFFERENCE BETWEEN LOCAL AND REMOTE META STORES?**

Local meta stores run on the same Java Virtual Machine (JVM) as the Hive service whereas remote meta stores run on a separate, distinct JVM.

**WHAT IS THE DEFAULT APACHE HIVE METASTORE DATABASE?**

The default database for metastore is the embedded Derby database provided by Hive which is backed by the local disk.

**CAN MULTIPLE USERS USE ONE METASTORE?**

It depends upon underlying database for metastore. 

If it is derby then no because debrly allow only one connection at a time else yes.


# ****Engine****

# ****HDFS****

# ****YARN****




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
