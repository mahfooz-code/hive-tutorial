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

**WHAT ARE THE LIMITATIONS OF HIVE?**

Hive has the following limitations: 

1) Real-time queries cannot be executed and it has no row-level support.
2) Hive cannot be used for online transaction processing.


# ***************************HIVE ARCHITECTURE***************************

**WHAT ARE THE DIFFERENT COMPONENTS OF A HIVE ARCHITECTURE?**

Following are the five components of a Hive Architecture:

- **User Interface**: It helps the user to send queries to the Hive system and other operations. The user interface provides hive Web UI, Hive Command-Line and Hive HDInsight.
- **Driver**: It designs a session handle for the query, and then the queries are sent to the compiler for the execution plan.
- **Metastore**: It contains the organized data and information on various warehouse tables and partitions.
- **Compiler**: It creates the execution plan for the queries, performs semantic analysis on different query blocks, and generates query expression.
- **Execution Engine**: It implements the execution plans created by the compiler. 


# ****Client****

**What applications are supported by Hive?**

Hive supports client applications based on Java, PHP, Python, C, and Ruby coding languages.



# ****Hive cli****

**IS IT POSSIBLE TO RUN UNIX SHELL COMMANDS IN HIVE?**

Yes, one can run shell commands in Hive by adding a '!' before the command.

**IS IT POSSIBLE TO EXECUTE HIVE QUERIES FROM A SCRIPT FILE?**

Yes, one can do so with the help of a source command.

1) Hive> source /path/queryfile.hql
2) hive -f /path/queryfile.hql

**What is .hiverc file?**

It is a file that is executed when you launch the hive shell - making it an ideal place for adding any hive configuration/customization you want set, on start of the hive shell. 
This could be:
- Setting column headers to be visible in query results
- Making the current database name part of the hive prompt
- Adding any jars or files
- Registering UDFs

# ****Hive Web Interface****

# ****Thrift server****

# ****Hive jdbc****

# ****Modes****

**WHAT ARE THE THREE DIFFERENT MODES IN WHICH HIVE CAN BE OPERATED?**

The three modes in which Hive can be operated are **Local mode, distributed mode, and pseudo-distributed mode.**


# ****Data model****

**WHAT ARE THE DIFFERENT TABLES AVAILABLE IN HIVE?**

There are two types of tables available in Hive – managed and external.

**IS THERE A DATA TYPE IN HIVE TO STORE DATE INFORMATION?**

The TIMESTAMP data type in Hive stores all data information in the java.sql.timestamp format.

**WHY IS PARTITIONING USED IN HIVE?**

Partitioning is used in Hive as it allows for the reduction of query latency. 

Instead of scanning entire tables, only relevant partitions and corresponding datasets are scanned.

**WHAT ARE THE HIVE COLLECTION DATA TYPES?**

ARRAY, MAP, AND STRUCT are the three Hive collection data types.

**HOW CAN YOU CHECK IF A SPECIFIC PARTITION EXISTS?**

Use the following command: _SHOW PARTITIONS table_name PARTITION (partitioned_column='partition_value')_

**IS IT POSSIBLE TO DELETE DBPROPERTY IN HIVE?**

No, there is no way to delete the DBPROPERTY.


**WHEN A HIVE TABLE PARTITION IS POINTED TO A NEW DIRECTORY, WHAT HAPPENS TO THE DATA?**

The data remains in the old directory and needs to be transferred manually. 


**DO YOU SAVE SPACE IN THE HDFS BY ARCHIVING HIVE TABLES?**

No, archiving Hive tables only helps reduce the number of files that make for easier management of data.

**WHAT IS A VIEW IN HIVE?**

A view is a logical construct that allows search queries to be treated as tables.

**CAN THE NAME OF A VIEW BE THE SAME AS A HIVE TABLE NAME?**

No, the name of the view must always be unique in the database.

**CAN WE USE THE LOAD OR INSERT COMMAND TO VIEW?**

No, these commands cannot be used with respect to a view in Hive.

**WHAT IS INDEXING IN HIVE?**

Hive indexing is a query optimization technique to reduce the time needed to access a column or a set of columns within a Hive database.

**ARE MULTI-LINE COMMENTS SUPPORTED BY HIVE?**

No, multi-line comments are supported by Hive.

**WHAT IS BUCKETING?**

Bucketing is the process of hashing the values in a column into several user-defined buckets which helps avoid over-partitioning.

**HOW IS BUCKETING HELPFUL?**

Bucketing helps optimize the sampling process and shortens the query response time.


**CAN YOU SPECIFY THE NAME OF THE TABLE CREATOR IN HIVE?**

Yes, by using the TBLPROPERTIES clause. For example – TBLPROPERTIES (‘creator’= ‘john’)

**WHAT IS UDF IN HIVE?**

UDF is a user-designed function created with a Java program to address a specific function that is not part of the existing Hive functions.

**HOW HIVE DISTRIBUTES THE ROWS INTO BUCKETS?**

Hive uses the formula: hash_function (bucketing_column) modulo (num_of_buckets) to calculate the row's bucket number. 
Here, hash_function is based on the Data type of the column. 

The hash_function is for integer data type:

hash_function (int_type_column)= value of int_type_column


# *******************************HIVE SERDE*******************************

**WHICH JAVA CLASS HANDLES THE INPUT RECORD ENCODING INTO FILES THAT STORE HIVE TABLES?**

The 'org.apache.hadoop.mapred.TextInputFormat' class.

**WHICH JAVA CLASS HANDLES OUTPUT RECORD ENCODING INTO HIVE QUERY FILES?**

The 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat' class.


**HOW DO ORC FORMAT TABLES HELP HIVE TO ENHANCE THE PERFORMANCE?**

You can easily store the Hive Data with the ORC (Optimized Row Column) format as it helps to streamline several limitations.


# ******************************HCATALOG******************************

**WHAT IS HCATALOG?**

Hcatalog is a tool that helps to share data structures with other external systems in the Hadoop ecosystem.

**WHY DO YOU NEED A HCATOLOG?**

For sharing Data structures with external systems, Hcatalog is a necessary tool.

It offers access to the Hive metastore for reading and writing data in a Hive data warehouse.

# *****************************SQL*****************************

**IF YOU HAD TO LIST ALL DATABASES THAT BEGAN WITH THE LETTER 'C', HOW WOULD YOU DO IT?**

By using the following command: _SHOW DATABASES LIKE 'c.*'_

**HOW CAN YOU STOP A PARTITION FROM BEING ACCESSED IN A QUERY?**

Use the ENABLE OFFLINE clause along with the ALTER TABLE command.


**CAN A CARTESIAN JOIN BE CREATED BETWEEN TWO HIVE TABLES?**

This is not possible as it cannot be implemented in MapReduce programming.


**HOW CAN YOU VIEW THE INDEXES OF A HIVE TABLE?**

By using the following command: SHOW INDEX ON table_name


**WHAT IS THE HIVE OBJECTINSPECTOR FUNCTION?**

It helps to analyze the structure of individual columns and rows and provides access to the complex objects that are stored within the database.

**WHAT DOES /*STREAMTABLE(TABLE_NAME)*/ DO? **

It is a query hint that allows for a table to be streamed into memory before a query is executed.

# ****Driver****

**NAME THE COMPONENTS OF A HIVE QUERY PROCESSOR?**

Following are the components of a Hive query processor:

- Logical Plan of Generation.
- Physical Plan of Generation.
- Execution Engine.
- UDF’s and UDAF.
- Operators.
- Optimizer.
- Parser.
- Semantic Analyzer.
- Type Checking


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

**WHAT IS A TABLE GENERATING FUNCTION ON HIVE?**

These functions transform a single row into multiple rows. 

EXPLODE is the only table generated function. 

This function takes array as an input and outputs the elements of array into separate rows.

# ****Engine****

**CAN YOU AVOID MAPREDUCE ON HIVE?**

You can make Hive avoid MapReduce to return query results by setting the hive.exec.mode.local.auto property to ‘true’.


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
