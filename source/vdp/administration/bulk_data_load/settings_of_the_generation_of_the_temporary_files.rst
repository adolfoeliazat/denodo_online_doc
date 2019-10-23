=================================================
Settings of the Generation of the Temporary Files
=================================================

When Virtual DataPort uses the proprietary interface of the database to
load data in bulk, first, it writes the data to a delimited file that is
deleted once is transferred to target database.

The following properties define several aspects of these temporary
delimited files:

 .. table:: Settings of the generation of the temporary files for bulk data load
    
   +--------------------------------------+---------------------------------------------------------------------------------+
   | Meaning                              | Property name                                                                   |
   +======================================+=================================================================================+
   | Encoding of the file.                | com.denodo.vdb.util.tablemanagement.sql.insertion.DFInsertWorker.encoding       |
   |                                      |                                                                                 |
   | Default encoding: UTF-8              |                                                                                 |
   +--------------------------------------+---------------------------------------------------------------------------------+
   | Separator of values.                 | com.denodo.vdb.util.tablemanagement.sql.insertion.DFInsertWorker.fieldSeparator |
   |                                      |                                                                                 |
   | Default value: 0x1E (Information     |                                                                                 |
   | separator two - U+001E)              |                                                                                 |
   +--------------------------------------+---------------------------------------------------------------------------------+
   | Quote character.                     | com.denodo.vdb.util.tablemanagement.sql.insertion.DFInsertWorker.quoteCharacter |
   |                                      |                                                                                 |
   | Default value: 0x1F (Information     |                                                                                 |
   | separator one - U+001F)              |                                                                                 |
   +--------------------------------------+---------------------------------------------------------------------------------+
   | Row separator                        | com.denodo.vdb.util.tablemanagement.sql.insertion.DFInsertWorker.rowSeparator   |
   |                                      |                                                                                 |
   | Default value: 0x1E followed by the  |                                                                                 |
   | end of line of the platform where    |                                                                                 |
   | the Virtual DataPort server is       |                                                                                 |
   | running.                             |                                                                                 |
   +--------------------------------------+---------------------------------------------------------------------------------+

To change its value, use the administration tool to login with an
administrator account and from the VQL Shell, execute this:

 

.. code-block:: sql

   SET '<name of the property>' = '<new value>';

For example:

.. code-block:: sql

   SET 'com.denodo.vdb.util.tablemanagement.sql.insertion.DFInsertWorker.encoding' 
       = 'UTF-16';

Additionally, you can change these settings for a specific database
adapter instead of for all of them. That is, you can change the encoding
of the files that will be written in Oracle, but leave the default
encoding for the rest.

To do this, add the name of adapter after ``DFInsertWorker.``, followed
by a dot. For example,

 

.. code-block:: sql

   SET 'com.denodo.vdb.util.tablemanagement.sql.insertion.DFInsertWorker.oracle.encoding' 
       = 'UTF-16';

The name of the adapter is the value of the parameter ``DATABASENAME``
of the ``CREATE DATASOURCE JDBC`` statement that created the data
source. You can check this name in the dialog “VQL” of the data source.

Databases with HDFS Storage
===========================

For the databases that use HDFS storage (Hive, Impala, Presto and Spark) the 
temporary files are generated using the Parquet file format.

Some options of the format may be tuned using the following options.

Compression
-----------

The temporary files are generated without compressing them. If you want Parquet
files to be compressed, execute this command from the VQL Shell:

.. code-block:: vql

   SET 'com.denodo.vdb.util.tablemanagement.sql.insertion.HdfsInsertWorker.parquet.compression' 
       = '<compression mechanism>';

The value of "<compression mechanism>" can be ``off`` (no compression), ``snappy``, ``gzip`` or ``lzo``.

You do not need to restart after changing this property.

The database needs to support the selected compression algorithm.

Generating these files compressed may speed up the process loading the cache of a view or a data movement.
However, if the database is in the same local area network as the Denodo server, it may not be the case 
because the transfer speed is high. It is possible that there is not any reduction in time and an increase
of CPU usage by the Denodo server because it has to generate the files compressed.

Decimal Precision and Scale
---------------------------

In Parquet files, the precision and scale of the values of type "decimal" has to be specified.

The Denodo server uses the "source type properties" of the fields - if available - to set these values. 
However, the source type properties are not always available (e.g. fields obtained from non-JDBC data sources
do not have these properties unless the user manually sets them in the base view, by default the result of an expression does not have these properties, etc.).
When these properties are not available, the Server assumes that the precision
is 38 and the scale 20. To change these values, execute the following
commands from the VQL Shell (only administrators are allowed to execute them):

.. code-block:: vql

   SET 'com.denodo.vdb.util.tablemanagement.sql.insertion.HdfsInsertWorker.decimal.parquet.decimal.precision' 
       = '<precision>';

   SET 'com.denodo.vdb.util.tablemanagement.sql.insertion.HdfsInsertWorker.decimal.parquet.decimal.scale' 
       = '<scale>';

When a value could not be represented using the current precision and scale an error like 
``Error processing value "xxx" as precision y and scale z`` is shown.
