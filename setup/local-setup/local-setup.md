1) Download Hive from Apache Hive and unpack it:
cd /opt
wget https://archive.apache.org/dist/hive/hive-2.3.3/apache-hive-2.3.3-bin.tar.gz
tar -zxvf apache-hive-2.3.3-bin.tar.gz
ln -sfn /opt/apache-hive-2.3.3 /opt/hive 

2) Add the necessary system path variables in the ~/.profile or ~/.bashrc file:
export HADOOP_HOME=/opt/hadoop
export HADOOP_CONF_DIR=/opt/hadoop/conf
export HIVE_HOME=/opt/hive
export HIVE_CONF_DIR=/opt/hive/conf
export PATH=$PATH:$HIVE_HOME/bin:$HADOOP_HOME/
bin:$HADOOP_HOME/sbin

3) Enable the settings immediately:
source ~/.profile

4) Create the configuration files:
      $cd /opt/hive/conf
      $cp hive-default.xml.template hive-site.xml
      $cp hive-exec-log4j.properties.template hive-exec-
      log4j.properties
      $cp hive-log4j.properties.template hive-log4j.properties

5) Modify $HIVE_HOME/conf/hive-site.xml, which has some important parameters to set:

	hive.metastore.warehourse.dir: This is the path to the Hive warehouse location. By default, it is at /user/hive/warehouse.
	hive.exec.scratchdir: This is the temporary data file location. By default, it is at /tmp/hive-${user.name}.

6) 

The following is a sample setting using MySQL as the metastore database:

<property>
  <name>javax.jdo.option.ConnectionURL</name>
  <value>jdbc:mysql://localhost/metastore?createDatabaseIfNotExist=true
  </value>
  <description>JDBC connect string for a JDBC metastore</description>
</property>
<property>
  <name>javax.jdo.option.ConnectionDriverName</name>
  <value>com.mysql.jdbc.Driver</value>
  <description>Driver class name for a JDBC metastore</description>
</property>
<property>
  <name>javax.jdo.option.ConnectionUserName</name>
  <value>hive</value>
  <description>username to use against metastore database</description>
</property>
<property>
  <name>javax.jdo.option.ConnectionPassword</name>
  <value>mypassword</value>
  <description>password to use against metastore database</description>
</property>
<property>
  <name>hive.metastore.uris</name>
  <value>thrift://localhost:9083</value>
  <description>By specify this we do not use local mode of metastore</description>
</property>

7) Make sure that the MySQL JDBC driver is available at $HIVE_HOME/lib:
      $ln -sfn /usr/share/java/mysql-connector-java.jar 
      /opt/hive/lib/mysql-connector-java.jar
