# Hive Cli 

Hive is an Apache-Thrift-based client

The hive command directly connects to the Hive drivers, so we need to install the Hive library on the client.

# Connecting to hive server
hive -h hostname -p port

# Running hive query in batch mode

hive -e "hql query"

# Running hive script file

hive -f hql_query_file.hql

hive -i hql_init_file.hql

# Setting variable

hive --hivevar var_name=var_value


# Run dfs cmd

dfs -ls;

# Run hql file

source hql_query_file.hql




