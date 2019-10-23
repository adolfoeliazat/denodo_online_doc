===============
Amazon Redshift
===============

Redshift provides two method of uploading to Redshift the data file that
Virtual DataPort has generated:

1. Upload the data file to an S3 bucket and from there, to the database (recommended method).

   To use this method, in the **Read & Write** tab of the data source,
   enter the values of **AWS access key id**, **AWS secret access key** and
   **S3 bucket name**. Leave empty the boxes whose label starts with “SSH”.

#. Alternatively, grant access to Redshift to connect via SSH to a host
   where it can find the data file. This method can only be used if the
   host where the data file is stored is Linux.

   To use this method, in addition to provide the same values as in the
   previous method, enter a value for the boxes below (the ones starting
   with “SSH”):
   
   -  **SSH endpoint**: host to which the Redshift instance will connect to
      retrieve the data file.
   -  **SSH work path**: directory where the Redshift instance will locate
      the data file.
   -  **SSH user name**: user name that the Redshift instance will use to
      connect to this host.
   -  **SSH public key**: public key that the Redshift instance will use to
      connect to this host. Note that you will have to configure the host
      to allow connections with this public key; and not with a user name
      and password.

When the Denodo server is the one that uploads the data to Amazon Redshift,
the process is more efficient than with other adapters. The data is uploaded 
in chunks and in parallel instead of generating a single file with all the data 
and once is completely written to disk, transfer it to Redshift.

Let us say that you execute a query to load the cache of a view and that the cache
database is Redshift. The process of loading the cache is the following:

1. The Server executes the query and begins to obtain the rows it has to store in the cache database; as soon as it obtains the first rows of the result, it starts writing them to the data file. This file is written to disk compressed.

2. Once the Server writes 500,000 rows into the file, it closes the file and begins sending it to Redshift. Simultaneously, it writes the next rows into another file.
   
   500,000 is the default value of the field “Batch insert size (rows)” of the “Read & Write” tab of the data source and it can be changed. Note that increasing it will reduce the parallelism of the upload process.

3. When the Server finishes writing another data file (i.e. the files reach the batch insert size), it begins to 
   transfer it to Redshift even if the files generated previously have not been completely transferred. By default, 
   the Server transfers up to 4 files concurrently per query. When it reaches this limit, the Server keeps writing 
   rows into the data files, but does not transfer more files. 

   This limit can be changed by executing the following command from the VQL Shell:
 
   .. code-block:: sql
   
      SET 'com.denodo.vdb.util.tablemanagement.sql.insertion.DFInsertWorker.
      redshift.maxS3ParallelUploads' = '<new limit per query>';

4. Once a data file is sent, it is deleted.

5. Once all the files are uploaded, the Server instructs Redshift to load these files.

.. note:: If you configure the data source to use the second alternative (i.e. Redshift connects to one of your hosts and reads the data files), the explanation above does not apply. Instead, what happens is that Virtual DataPort begins writing data to disk. Once the query finishes and all the data is written to disk, it instructs Redshift to load these files. This approach is slower than the first approach (the recommended one), especially when dealing with hundreds of millions of rows, because Redshift does not begin to retrieve data until the query finishes and all the data is written on the files. With the first approach Virtual DataPort begins uploading data as soon as the first file is completed (i.e. the file contains the number of rows indicated in the field “Batch insert size”).
