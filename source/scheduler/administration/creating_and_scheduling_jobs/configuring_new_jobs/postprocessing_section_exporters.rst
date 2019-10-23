==================================
Postprocessing Section (Exporters)
==================================

In Denodo Scheduler different post processing actions can be defined on
the tuples obtained (except in :ref:`VDPCache <VDPCache Extraction Section>` jobs, which do not have a postprocessing section). 


Each post-processing action will be composed of one
*filter sequence* and one *exporter* *of results*. The filter sequence
has to be created in advance (see section :doc:`../filter_sequences/filter_sequences`). It allows using
different criteria to filter/transform the tuples that will be sent to
the different exporters.


This section has a global option **Export as transaction**, which only affects JDBC exporters. 
When enabled, the export to all JDBC exporters (if any) is treated as a single transaction. 
This means that all target tables will contain the same data once the job is finished. 
If an error occurs in any of them, the export to all JDBC exporters (the other types are not affected) 
is rolled back and each one returns to the state in which it was at the beginning of the execution. 
Only when there is no error, the export with the new data is committed in JDBC exporters.

The following are different exporters that Denodo Scheduler considers,
along with a small description of use for each of them.


Exporters That Can Only Be Used In VDPIndexer Jobs
-------------------------------------------------------

-  **Scheduler Index**. Stores the extracted tuples in an index of the Scheduler
   indexing server. To configure this exporter the following parameters need
   to be specified:

   -  **Data source**. For selecting the Scheduler index data source associated
      to the index to which the tuples/documents will be exported.
   -  **Index name**. Name of the Scheduler Index server index in which the
      tuples/documents will be stored.
   -  **Clear index**. If checked, before proceeding to store new documents
      in the specified index, all documents associated with the identifier
      of the current job will be deleted.

   .. note:: Until this version of Denodo Scheduler, these exporters 
      were named as *ARN-Index*.
      
-  **Elasticsearch**. Stores the extracted documents in an index of the
   Elasticsearch indexing server. To configure this exporter the following
   parameters need to be specified:

   -  **Data source**. For selecting the Elasticsearch data source associated
      to the index to which the tuples/documents will be exported.

   -  **Index name**. Name of the index where the tuples/documents will be
      stored.

   -  **Clear index**. If checked, before proceeding to store new documents in
      the specified index, all documents associated with the identifier of the
      current job will be deleted.

   -  **Recreate index**. If checked, it deletes the selected index and all
      its documents. Then creates again the index with the configuration
      specified in the following parameter.

   -  **Index Configuration**. For specifying the parameters to create the
      index. These parameters only have effect when creating the index. If the
      index already exists and ‘Recreate Index’ is not checked, they will have
      no effect.

      -  **Shards**. Number of primary shards the index is composed of. A
         shard is a single Lucene instance managed automatically by
         Elasticsearch. By default, an index has 5 primary shards,
         distributed amongst all nodes in the cluster.
      -  **Replicas**. Number of copies of the primary shards in the cluster.
         By default, there is no replication.
      -  **Analyzer**. The analyzer to be used should be selected in
         accordance with the language expected for the documents. Stopwords
         (those words that are very common in the language) will be removed.

   -  **Bulk processor configuration**. In order to process high volume of
      data, the exporter groups the requests (documents to be indexed or
      deleted) in batches and sends all the requests of each batch to the
      Elasticsearch server at once. There are several parameters to configure
      when to send a batch:

      -  **Document chunk size**. Number of documents that make up a chunk of
         results. When this number of documents (to be indexed of deleted) is
         reached, the exporter will send them to server, even though the other
         parameters have not been reached (by default 10000).
      -  **Chunk size (MB)**. Size of the documents that make up a chunk of
         results. When the size of the documents to be indexed or deleted
         reaches this size, the exporter sends them to the server, even though
         the other parameters have not been reached (by default 50 MB).
      -  **Flush interval**. Maximum time (in seconds) that the exporter will
         wait for sending the pending requests to the server. When it reaches
         this timeout, it will send the pending requests to the server, even
         though the other parameters have not been reached (by default
         60 seconds).
      -  **Concurrency level**. Sets the number of concurrent requests allowed
         to be executed by the exporter, that is, the number of batches that
         can be sent to the server concurrently (by default 5).


Exporters That Can Only Be Used In Non-VDPIndexer Jobs
-------------------------------------------------------

-  **CSV**. Stores the tuples obtained in a plain text file delimited by
   the character specified in the **Separator** parameter. The user can
   specify in the **Output file** field the path and the name of the
   generated file. By default, it is generated in the directory
   :file:`{<DENODO_HOME>}/work/scheduler/data/csv` with the name
   ``<PROJECT_NAME>_<JOB_NAME>_<JOB_ID>_Group#<GID>_CSVExporter#<EID>_<JOB_START_TIME>.csv``,
   JOB\_START\_TIME being the moment of execution of the job expressed in
   milliseconds since January 1, 1970. Checking the **Include header**
   check box includes a first line in the file with the name of the fields
   of each exported tuple. If the **Overwrite to old file** radio button is
   ticked, the file name will be generated without information of the
   moment of execution (without the JOB\_START\_TIME), so that multiple
   executions of the job will overwrite the same file instead of generating
   new files. Otherwise, if the **Create new file** radio button is
   checked, each execution will create a new file (by adding the
   JOB\_START\_TIME to the file name). Finally, if the **Append to old
   file** radio button is checked, the exported data will be appended to
   the existing file. Use the **Encoding** drop-down to select the encoding
   of the text file (with support for generating a file with BOM - `Byte Order Mark <http://www.unicode.org/faq/utf_bom.html>`_ -
   for the following
   encodings: UTF-8, UTF16-LE, UTF16-BE, UTF-32LE, UTF-32BE and GB-18030).
   Use the **i18n** drop-down in order to format dates and numbers
   according to the selected one. It is also possible to specify
   associations between the field names in the tuples obtained by the job
   (parameter **Document field**) and the actual names of fields in the CSV
   file (**CSV column**, this parameter is case sensitive). If
   associations have been specified, the **Export only mapped attributes**
   check box allows to export those fields only. Finally, if you want to
   include the project name, job name job identifier and job start time
   among the fields for the exported tuples, it is necessary to check the
   last check box (**Export job identifier, job name project name and
   execution time fields**).


   .. note:: You can use the CSV exporter to generate files that can be
      directly opened with Microsoft Excel. In order to do that, you just need to 
      specify ‘;’ as separator.


-  **JDBC**. Stores the tuples in a relational database table. To configure
   it a JDBC-type **Data source** needs to be specified that accesses the
   relational database containing the table. The optional parameter
   **Schema** allows specifying the existing schema where the table is
   stored. The **Table name** parameter (case sensitive) specifies an
   existing table where the tuples will be inserted. By default, the
   destination table schema must have a field with a compatible data type
   for each job field. It is also possible to specify associations between
   the field names in the tuples obtained by the job (parameter **Document
   field**) and the actual names of fields in the database table (**Table
   column**, this parameter is case sensitive). If associations have
   been specified, the **Export only mapped attributes** check box allows to
   export those fields only. You can also include the project name, job
   name, job identifier and job execution time among the fields of the
   exported tuples by checking the **Export job identifier**, **job name**,
   **project name** and **execution time fields’** check box). If you want
   the content of the table to be deleted prior to inserting the exported
   tuples, you can check the **Delete table content** option (in this case,
   the content of the table is deleted at the beginning of the job
   execution, not at the beginning of the exportation phase). By default,
   the JDBC exporter performs insertions, so if another tuple with the same
   values for the primary key already exists, it will result in an error.
   By checking the **Update tuple if entry already exists in DB** option,
   the JDBC exporter will update the tuple instead of inserting it
   (compound primary keys are allowed). Besides, if this option is not
   checked, the chunk (whose size is determined by the parameter **Batch
   size**) of documents received by the exporter is inserted in the data
   base as a batch (using JDBC Batch Inserts). You can also configure
   **Error Management**. There are three options:


   -  **Never rollback**. A rollback will never be performed regardless of
      any errors in the exportation. Every chunk of documents is exported
      on its own transaction and the deletion of the previous content of
      the table (if that option is used) is also executed in one different
      transaction. At the end of each chunk, successfully inserted/updated
      tuples will be permanently stored in the database.
      
   -  **Rollback if the exportation of all documents fails**. A rollback
      will be performed when all documents fail to be exported. The same
      transaction is used to export all the documents and to delete the
      previous content of the table (if that option is used).
      
   -  **Rollback if any exportation error occurs**. Same as above, but a
      rollback is executed if an error occurs while deleting the previous
      content of the table (if that option is used), or the exportation of
      any document fails.

   .. note:: If **Export as transaction** is enabled, the **Error Management** 
      options of each individual JDBC exporter are overwritten. 

-  **SQL**. The SQL exporter functionality is similar to that of the JDBC
   exporter, but instead of inserting tuples in a relational database it
   generates a text file with corresponding sql INSERT statements. This
   way, the generated file can be used to load the data in a database at a
   later time. Unlike the JDBC exporter, it does not require a **Data
   source** to be specified, but it is possible to specify a database
   adapter (**Database adapter**) to take into account the differences in
   the SQL syntax used by different databases (currently a special adapter
   for Oracle and another for SQL Server are supported). The user can
   specify in the **Output file** field the path and the name of the
   generated file. By default, it will be generated in the
   ``DENODO_HOME/work/scheduler/data/sql`` directory with the name
   following the convention
   ``<PROJECT_NAME>_<JOB_NAME>_<JOB_ID>_Group#<GID>_SQLExporter#<EID>_<JOB_START_TIME>.sql``,
   JOB\_START\_TIME being the moment the job is executed expressed in
   milliseconds since January 1, 1970. If the **Overwrite to old file**
   radio button is ticked, the file name will be generated without
   information of the moment of execution (without the JOB\_START\_TIME),
   so that multiple executions of the job will overwrite the same file
   instead of generating new files. Otherwise, if the **Create new file**
   radio button is checked, each execution will create a new file (by
   adding the JOB\_START\_TIME to the file name). Finally, if the **Append
   to old file** radio button is checked, the exported data will be
   appended to the existing file. Use the **Encoding** drop-down to select
   the encoding of the text file (with support for generating a file with
   BOM - `Byte Order Mark <http://www.unicode.org/faq/utf_bom.html>`_ - 
   for the following
   encodings: UTF-8, UTF16-LE, UTF16-BE, UTF-32LE, UTF-32BE and GB-18030).
   
-  **Custom Exporters**. Users can also create their own custom exporters. 
   See section :ref:`Exporters`.
   
-  **File-Repository**. 
   
   .. important:: This exporter is deprecated and will be removed in a later version of the Denodo Platform.
      No new "*File-Repository*" exporters can be created in version 7.0, but you can edit and use the existing ones. 
      You can see the documentation from `previous version <https://community.denodo.com/docs/html/browse/6.0/scheduler/administration/creating_and_scheduling_jobs/configuring_new_jobs/postprocessing_section_exporters>`_.
   
      The section :ref:`Features Deprecated in Scheduler 7.0` lists all the features that are deprecated.
   
.. note:: The Scheduler Index exporter does not support indexing of binary
   fields. In the case of CSV and SQL exporters, binary fields are exported
   as text encoded in base64.

.. note:: With regard to compound data types, they are exported in text,
   formatted in XML.

.. note:: If using a JDBC exporter and the exportation of a tuple fails,
   the process will continue with the remaining ones of the block, unless
   the exception is applicable for the whole set of tuples. In this case,
   they will be discarded.

   The report of the job will show the document identifier (tuple number)
   that failed and the reason.

.. note:: About CSV and SQL exporters:

   -  The resultant files can be **encrypted** (using the MD5-DES cipher algorithm) or **compressed** (ZIP file).
   -  Besides the standard variables (``projectName``, ``jobName``, ``jobID`` and ``jobStartTime``), it is
      possible to specify any variable (following the syntax
      ``@{field:<DOCUMENT_FIELD_NAME>}``) in the **Output file** parameter,
      where ``DOCUMENT_FIELD_NAME`` represents a field in the exported
      document. This way, according to the value of these variables, the
      exported documents will go to a file or to another one (the file name
      will depend on the values of the fields of the exported document). It is
      also possible to format date variables by specifying input and output
      date patterns (input pattern is optional; if not specified, it assumes
      the date in milliseconds). The syntax is
      ``@{jobStartTime, outputDateFormat:<output_format>}`` for the standard
      variable ``jobStartTime`` and
      ``@{field:<DOCUMENT_FIELD_NAME>,inputDateFormat: <input_format>,outputDateFormat: <output_format>}``
      for any other date field of the exported document, where, for instance,
      ``inputDateFormat`` could be ``yyyyMMdd`` and
      ``outputDateFormat “yyyy-MM-dd”``.
   -  By default, if no tuples are exported, the output files are deleted (unless they already exist from previous executions).
      If you want to keep the file even though it is empty, check the **Create file even if no tuples are exported** field.

.. note:: When exporting data to ODBC targets, the JDBC exporter used
   must have a data source configured with the “JDBC-ODBC Bridge” driver.
   If the target is an Excel sheet, the data source must be configured with
   the “Excel” adapter and the **Table name** field must be filled in with
   “[Sheet1$]”. Both drivers are distributed with Denodo Scheduler (see
   table :ref:`JDBC Drivers <scheduler_administration_guide_JDBC_Drivers_Supported_by_Scheduler>`). When working with Excel sheets it is important to take
   into account some limitations (see
   https://support.office.com/en-us/article/Excel-specifications-and-limits-1672b34d-7043-467e-8e27-269d656771c3).

.. note:: Consecutive exporters using the same filter sequence are
   grouped together. It is the recommended way of adding exporters to a
   job, in order to execute the filter sequence once for the whole group.
   If two exporters share the same filter sequence, but they are not
   consecutive (other exporters with a different filter sequence has been
   created between them) then they will not be in the same group.

