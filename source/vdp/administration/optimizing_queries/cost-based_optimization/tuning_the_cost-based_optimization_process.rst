==========================================
Tuning the Cost-Based Optimization Process
==========================================

Accurately estimating costs is the most crucial point of the
optimization process. This section provides an overview of how Virtual
DataPort estimates costs and some tips for achieving better estimations.

A query plan is composed of one or several queries that are pushed-down
to the data sources and a series of post-processing operations that
combine and transform the results of the different data source queries.
Therefore, to estimate the cost of a plan we need to consider two types
of costs:

-  The cost of executing a query on a data source, and transferring the
   results through the network from the data source to Virtual DataPort.
-  The cost of the post-processing operations performed by the execution
   plan.

The next two subsections respectively explain the basics of how Virtual
DataPort estimates each one of these cost components and what
information needs to achieve good estimations. The following sections
explain in more detail how to provide such information in the specific
cases where it is not able to acquire it automatically.

.. contents:: 
   :depth: 1
   :local:
   :backlinks: none


Estimating the Cost of Queries on Data Sources
=================================================================================

The cost of executing a subquery on a data source has two main
components:

-  The estimated cost of processing the query in the data source. The
   main factor taken into account is the estimated number of I/O
   operations performed by the data source.
-  The estimated cost of transferring the obtained results through the
   network. The main factor influencing this cost is the size of the
   result set.

Therefore, to generate cost-estimations, Virtual DataPort needs the
following:


-  *View statistics*. To achieve good estimations of both the required
   number of I/O operations and the network transfer cost, it is crucial to
   have available at least the following statistics for the view used in
   the data source query:

   -  Number of total rows of the view
   -  Field statistics of all the fields of the view that are used in the
      query. Note that a field might not be referenced in the actual query
      performed, but may be required in a lower view.
   -  For these fields, it is mandatory to provide at least the number of
      distinct values.


-  *Available indexes for the query and their type*. Virtual DataPort
   examines the indexes defined in the view to determine if they apply to
   the query. Then, it uses different formulas to estimate the number of
   I/O operations depending on the index type.
   
   The number of I/O operations can change dramatically depending on if an
   index is applicable or not, and depending on the type of the index.
   Therefore, to achieve good estimations it is crucial to ensure the
   accuracy of the indexes information.
   
   The Server automatically introspects the available indexes and their
   types from most databases, but the administrator may need to modify
   this information in specific cases (see :ref:`Specifying Indexes` below)


-  *I/O parameters of the data source*. The number of I/O operations can be
   heavily influenced by some I/O parameters of the data source. Virtual
   DataPort provides adequate defaults for the most typical configurations
   in the most common types of data sources, but these parameters can be
   also modified if necessary (see section :ref:`Data Source I/O Parameters`).

Estimating Post-Processing Costs
=================================================================================

As Virtual DataPort obtains the results of the subqueries sent to data
sources, it transforms and combines them as specified by the query plan
to obtain the result.

To estimate this cost, Virtual DataPort uses formulas that depend on the
needed post-processing operations (e.g. joins, aggregations, selections,
etc.) and on the estimated number of rows (and their size) that each
operation has as input and produces as output.

Virtual DataPort requires having statistics for at least all the views
entirely pushed down to the data source (we call them *leaf* views) to
be able to apply cost-based optimization. Nevertheless, in some cases,
we recommend having the statistics of other derived views to achieve
estimations that are more accurate:

-  *Flatten views*. When a flattening operation is applied on a view,
   the schema of the resulting view may change dramatically. In
   addition, each row in the original view can produce many rows in the
   output view. Therefore, Virtual DataPort cannot estimate the
   statistics of the derived view from the statistics of the source
   view. Therefore, we recommend collecting the statistics of the
   derived view, following the same considerations as if it was a base
   view.
-  *Derived attributes*. When a view creates a new derived field from an
   expression (e.g. ``SELECT CONCAT(attr1, attr2) AS attr3 ...)``, we
   recommend gathering or specifying the statistics of the new field,
   especially when it will be frequently used in selection conditions.
   Otherwise, Virtual DataPort will assume default values for the
   statistics.



Specifying Indexes
=================================================================================

You can declare indexes over base views that represent the indexes hold
in the data source. As explained in the section :ref:`Estimating the Cost of
Queries on Data Sources`, the cost-based optimization uses this
information to estimate the cost of the queries sent to data sources.

Virtual DataPort considers the following types of indexes:

-  **Clustered**. An index for a certain field is said to be clustered
   when the rows in the table are sorted in disk according to the field
   values. Clustered indexes reduce very significantly the cost of most
   queries where they can be applied.
-  **Hash**. A type of non-clustered index that can be only used with
   equality/inequality conditions but potentially provides faster access
   than other types of non-clustered indexes. In general, non-clustered
   indexes can enhance significantly the execution time for a query, but
   only when the number of returned rows is a small fraction of the
   total number of rows in the table
-  **Other**. Refers to other types of non-clustered indexes, such as
   B-Tree indexes.

When Virtual DataPort pushes down a query to a data source, it
determines if any of the indexes declared in the view is applicable to
the query and considers it to determine the estimated cost.

Especially with large tables, the type of index that is applicable for a
query may have a very big impact in its execution time. Therefore, it is
crucial for the information about indexes to be correct.

Virtual DataPort automatically introspects indexes from the supported
databases but sometimes, it may advisable to manually modify the
introspected information. If available, we also recommend adding
manually this information for data sources that are not databases. See
the next sections for details for specific databases and the section
:ref:`Defining an Index of a Base View` to know how to edit the index
information of a base view.

.. contents:: 
   :depth: 1
   :local:
   :backlinks: none

Tuning the Imported Indexes Information for Oracle
-----------------------------------------------------------------------------------------------------

In Oracle, there is not a clear distinction between clustered and
non-clustered indexes. All the indexes have a “clustering factor” that
indicates the degree to which the order of the rows in disk matches the
order of the values in the index.

Usually, in each table you will have one heavily clustered index that
can be considered as “clustered” for cost-optimization purposes. In many
cases, this is the index for the primary key fields.

*The JDBC Oracle driver indicates that all the indexes are clustered*.
Therefore, to achieve better cost estimations, you should manually
modify the indexes information of the base view, so the type of one of
the indexes is “Cluster” and the type of the others is “Other”.

There actually exists in Oracle a concept that is virtually equivalent
to clustered indexes: index organized tables. Indexes of tables of this
type will be correctly imported as “Cluster” indexes. In this case, no
further action is needed.



Tuning the Imported Indexes Information for MySQL
-----------------------------------------------------------------------------------------------------

In MySQL, every InnoDB table has one clustered index. If the primary key
exists in the table, this is the primary key index. If the table does
not define a primary key, MySQL picks the first UNIQUE index that only
has ``NOT NULL`` columns as the primary key (if it exists).

When a MySQL table is imported in Virtual DataPort, its primary key
index is considered as a “Cluster” index and no action is required.

If the table does not have primary key but it has a UNIQUE index that
verifies the restriction above, it will be imported as a non-clustered
index. In this case, you should manually modify the type of that index
and set it to “Clustered”.



Tuning the Imported Indexes Information for Informix
-----------------------------------------------------------------------------------------------------

In Informix, it is possible to “cluster” an index so the rows of the
table are stored in disk in the order specified by the index.
Nevertheless, the Informix JDBC driver exposes such indexes with the
type “Other”. We recommend changing the type of these indexes to
“Cluster”.



Tuning the Imported Indexes Information for Teradata
-----------------------------------------------------------------------------------------------------

Most index types of Teradata are imported correctly. Nevertheless, there
are two cases where you may want to modify the imported information:

-  Indexes of single table joins are not exposed by the Teradata’s JDBC
   driver and, therefore, they are not imported. If your tables have
   that type of indexes, we recommend declaring these indexes manually,
   with the type “Other”.
-  Unique Secondary Indexes in Teradata are imported with the type
   “Hash”. If you know that the fields of one of these indexes are used
   with equality conditions, we recommend changing the type of the index
   to “Clustered”.



Tuning the Imported Indexes Information for Netezza
-----------------------------------------------------------------------------------------------------

The concept of index does not exist in Netezza. Although there are some
data storage constructions that can be partially assimilated to them in
certain circumstances, the Netezza JDBC driver does not expose
information about them. Therefore, they will not be imported.

Nevertheless, we recommend adding the following indexes to the base
views imported from Netezza so to let Virtual DataPort know about some
similar data storage optimizations:

-  Declare one index of type “Cluster” for the distribution columns of
   the table (that is, the columns used to divide the data into
   partitions).
-  For views corresponding to clustered base tables in Netezza, declare
   in Virtual DataPort one index of type “Cluster” for the organizing
   keys of the clustered table.
   
   

Tuning the Imported Indexes Information for Redshift
-----------------------------------------------------------------------------------------------------

The concept of index does not exist in Redshift. Although there are some
data storage constructions that can be partially assimilated to them in
certain circumstances, the Redshift JDBC driver does not expose
information about them. Therefore, they will not be imported.

Nevertheless, we recommend adding the following indexes to the base
views imported from Redshift so to let Virtual DataPort know about some
similar data storage optimizations:

-  Declare a single index of type “Cluster” for the group of SORTKEY columns 
   of the Redshift table, when the SORTKEY is of type COMPOUND.  
   COMPOUND SORTKEY columns in Redshift indicate the physical order of rows 
   in disk. Therefore, their effect in cost estimation is similar to the 
   effect of conventional clustered indexes.
-  Declare one index of type "Other" for each SORTKEY column of type INTERLEAVED.
   For instance, if you have an INTERLEAVED SORTKEY with columns (col1, col2),
   declare one index of type 'Other' for 'col1' and another for 'col2'.
-  If the distribution key of your Redshift table is not part of the SORTKEY 
   columns, add an index of type 'Other' for it.    

Check the Redshift documentation for more information about SORTKEYs and distribution
keys.
   
   

Tuning the Imported Indexes Information for Greenplum
-----------------------------------------------------------------------------------------------------

The Greenplum JDBC driver does not export any information about indexes
so you should manually add the following index information for base
views imported from Greenplum databases:

-  Declare a clustered index for the distribution columns of the table
   (that is, columns used to divide the data).
-  Declare a clustered index for the columns used for defining Greenplum
   partitions.
-  Declare a clustered index for the columns participating in each
   Greenplum clustered index.
-  Declare an index of type “Other” for the columns participating in
   each Greenplum non-clustered index.



Tuning the Imported Indexes Information for Hive
-----------------------------------------------------------------------------------------------------

The Hive JDBC driver does not export any information about indexes.
Therefore, Virtual DataPort will not import any index information about
its tables. We recommend adding the following index information to the
base views imported from Hive tables:

-  Declare a clustered index for the distribution columns of the table
   (that is, columns used to divide the data into partitions).
-  Declare an index of type “Other” for the columns in each Hive index.



Tuning the Imported Indexes Information for Other Data Sources
--------------------------------------------------------------

The concept of index does not exist for other data sources such as Web
Services (REST or SOAP) or SAP BAPIs. Nevertheless, in many cases these
types of data sources act as a front-end to an underlying database. In
those cases, if you know that the underlying database specifies an index
for a certain column, we recommend adding such information to the
corresponding view.

For instance, the underlying databases behind many Web services used to
access data, contain an index for the columns associated with the
mandatory input parameters of the web service operations.





Data Source I/O Parameters
=================================================================================

The number of I/O operations performed by the data source to answer a
query can be heavily influenced by the value of some I/O parameters in
the data source. Therefore, the cost-based optimization takes into
account these parameters to obtain better estimations. The considered
parameters are:

-  *Block size*. The amount of data that is read or written in a single
   random I/O operation by the data source. In most databases, this
   value is in the range of 8 kilobytes to 16 kilobytes, although most of them allow
   changing it.
-  *Multi block read count*. Most databases read several blocks in the
   same I/O operation when those blocks are consecutive in disk (e.g.
   when performing a full scan on a table or reading an index). This
   parameter indicates how many consecutive blocks are read in a single
   I/O operation in those cases. The usual values are 8 or 9, although
   this is variable depending on the database and, in certain databases
   such as Oracle, can be manually changed by the database
   administrator.

Virtual DataPort provides adequate defaults for the most usual
configurations in the most common types of data sources. Nevertheless,
if your database use values significantly different from the default
values of the manufacturer, then it is advisable that you modify these
parameters accordingly in the corresponding Virtual DataPort data
source. To do this, edit the data source and in the “Source
configuration” tab, change the value of these properties.



