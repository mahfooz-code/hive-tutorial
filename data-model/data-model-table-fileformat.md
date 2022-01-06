#   Data optimization

    Data file optimization covers the performance improvement on the data files in terms of file format, 
    compression, and storage.
    Hive supports TEXTFILE, SEQUENCEFILE, AVRO, RCFILE, ORC, and PARQUET file formats.
    Once a table stored in text format is created, we can load text data directly into it.



#   Specifying the file format

#   CREATE TABLE ... STORE AS <file_format>: 
    Specify the file format when creating a table

#   ALTER TABLE [PARTITION partition_spec] SET FILEFORMAT <file_format>: 
    Modify the file format (definition only) in an existing table.


#   Text file format

    Once a table stored in text format is created, we can load text data directly into it.

    This is the default file format for table creation.
    Data is stored in clear text for this format.
    A text file is naturally splittable and able to be processed in parallel.
    It can also be compressed with algorithms, such as GZip, LZO, and Snappy.
    However, most compressed files are not splittable for parallel processing.
    As a result, they use only one job with a single mapper to process data slowly.
    The best practice for using compressed text files is to make sure the file is not too big and close to a couple of HDFS block sizes.

#   SEQUENCEFILE

    This is a binary storage format for key/value pairs.
    The benefit of a sequence file is that it is more compact than a text file and fits well with the MapReduce output format.
    Sequence files can be compressed to record or block level, where the block level has a better compression ratio.
    To enable block-level compression, we need use do the following settings: 
    
    set hive.exec.compress.output=true;
    set io.seqfile.compression.type=BLOCK;

#   AVRO
    This is also a binary format.
    More than that, it is also a serialization and deserialization framework. 
    AVRO provides a data schema that describes the data structure and also handles the schema changes, such as adding, renaming, and removing columns.
    The schema is stored along with data for any further processing. 
    
    Considering AVRO's advantages for dealing with schema evolution, it is recommended to use it when mapping the source data, which is likely to have schema changes time by time.

    


#   Setting file format for all or managed table
    
    To change the default file format for table creation property for all tables:

    SET hive.default.fileformat = <file_format> 
    
    Only for managed table:

    SET hive.default.fileformat.managed = <file_format>

#   Note
    TEXT, SEQUENCE, and AVRO files as a row-oriented file storage format are not optimal solutions since the query has to read a full row even if only one column is being requested. 

#   hybrid row-columnar storage

    A hybrid row-columnar storage file format, such as RCFILE, ORC, or PARQUET does not read to all row if specific column are requested.


