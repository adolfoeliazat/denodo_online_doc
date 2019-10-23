==============
Snowflake
==============

When uploading data to Snowflake, the process is more efficient than with other adapters. 
The data is uploaded in chunks and in parallel instead of generating a single file with
all the data and once is completely written to disk, transfer it to Snowflake. 

Let us say that you execute a query to load the cache of a view and that the cache database 
is Snowflake. The process of loading the cache is the following:

1. The Server executes the query and begins to obtain the rows it has to store in the cache database; as soon as it obtains the first rows of the result, it starts writing them to the data file. This file is written to disk compressed.
#. Once the Server writes 500,000 rows into the file, it closes the file and begins sending it to Snowflake. Simultaneously, it writes the next rows into another file.

   500,000 is the default value of the field “Batch insert size (rows)” of the “Read & Write” tab of the data source and it can be changed. Note that increasing it will reduce the parallelism of the upload process.

#. When the Server finishes writing another data file (i.e. the file reaches the batch insert size), it begins to transfer it to Snowflake even if the file generated previously has not been completely transferred. By default, the Server transfers up to 4 files concurrently per query. When it reaches this limit, the Server keeps writing rows into the data files, but does not transfer more files. 
    
   This limit can be changed by executing the following command from the VQL Shell:

   .. code-block:: sql
 
      SET 'com.denodo.vdb.util.tablemanagement.sql.insertion.DFInsertWorker.snowflake.maxParallelUploads' 
          = '<new limit per query>';
   
#.  Once a data file is sent, it is deleted.
#.  Once all the files are uploaded, the Server instructs Snowflake to load these files.
   