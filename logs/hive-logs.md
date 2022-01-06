#   Logs
    Logs provide detailed information to find out how a query/job runs.
    The system log contains the Hive running status and issues.
    It is configured in {HIVE_HOME}/conf/hive-log4j.properties.

#   To modify the logger level
    We can either modify the preceding property file that applies to all users, or set a Hive command-line config that 
    only applies to the current user session.
    
    hive --hiveconf hive.root.logger=DEBUG,console

#   application log using yarn
    
    yarn logs -applicationId <application_id>


