#   Locks
    
    Locks ensure data isolation as described in the ACID principle.
    Hive has supported concurrency access and locking mechanisms since v0.7.0 and updated to a new lock manager in v0.13.0. There are two types of lock provided as follows:

#   Shared lock:
    Also called S lock, it allows being shared concurrently.
    This is acquired when a table/partition is read. 

#   Exclusive lock:
    Also called X lock.
    This is acquired for all other operations that modify the table/partition.

    For partition tables, only a shared lock is acquired if the change is only applicable to the newly-created partitions.
    
    An exclusive lock is acquired on the table if the change is applicable to all partitions.
    
    In addition, an exclusive lock on the table globally affects all partitions.

#   Enable locking
    
    To enable locking, make sure the two properties are set in a Hive session or hive-site.xml.

    SET hive.support.concurrency = true
    SET hive.txn.manager = org.apache.hadoop.hive.ql.lockmgr.DbTxnManager

#   Show all locks when running merge into above
    SHOW LOCKS;





