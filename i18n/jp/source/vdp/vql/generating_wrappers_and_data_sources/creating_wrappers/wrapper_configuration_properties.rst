================================
Wrapper Configuration Properties
================================

Wrapper Configuration Properties allow indicating specific
characteristics of the underlying data sources such as their distributed
transaction support capacity or whether inserting operations are
permitted. The section :doc:`/vdp/vql/generating_wrappers_and_data_sources/creating_data_sources/data_source_configuration_properties` indicated the
configuration properties of the data sources. This section describes the
configurable properties in each wrapper, depending on the type of data
source they have come from.

.. note:: Typically, users do not need to edit this information since
   Virtual DataPort automatically uses suitable configurations for most common data
   sources.

The properties of each wrapper can be configured in the wrapper creation
statement by adding parameter/value pairs or from the Virtual DataPort
Administration Tool. The configurable properties are as follows:


-  **Allow Insert** (``ALLOWINSERT``). Configurable in JDBC and ODBC
   wrappers. It indicates whether the underlying data source supports
   ``INSERT`` operations and whether the wrapper will process them. The
   possible values are:

   -  ``default``: assigns the default value, which depends on the source.
   -  ``true``: the wrapper will process ``INSERT`` operations.
   -  ``false``: the wrapper will not process ``INSERT`` operations.
   
-  **Allow Delete** (``ALLOWDELETE``). Configurable in JDBC and ODBC
   wrappers. It indicates whether the underlying data source supports
   ``DELETE`` operations and whether the wrapper will process them. The
   possible values are:

   -  ``default``: assigns the default value, which depends on the source.
   -  ``true``: the wrapper will process ``DELETE`` operations.
   -  ``false``: the wrapper will not process ``DELETE`` operations.

-  **Allow Update** (``ALLOWUPDATE``). Configurable in JDBC and ODBC
   wrappers. It indicates whether the underlying data source supports
   ``UPDATE`` operations and whether the wrapper will process them. The
   possible values are:
   
   -  ``default``: assigns the default value, which depends on the source.
   -  ``true``: the wrapper will process ``UPDATE`` operations.
   -  ``false``: the wrapper will not process ``UPDATE`` operations.

-  **Delegate SQL sentence as subquery**
   (``DELEGATESQLSENTENCEASSUBQUERY`` ) Configurable in JDBC wrappers. If
   its value is ``yes`` and the base view has been created from a SQL
   query, at runtime, Virtual DataPort will be able to delegate more
   operations over the base view, to the database. To achieve this, the
   SQL query of the base view will be delegated as a subquery in the
   ``FROM`` clause.
   
   The section :ref:`View Configuration Properties` of the Administration
   Guide explains this property in more detail.

-  **Supports Distributed Transactions**
   (``SUPPORTSDISTRIBUTEDTRANSACTIONS``). Configurable in JDBC and ODBC
   wrappers. It indicates whether the underlying data source can take part
   in an XA distributed transaction. It is applicable
   to relational databases (accessible via JDBC and ODBC) and CUSTOM
   wrappers. The possible values are:

   -  ``default``: assigns the default value, which depends on the source.
   -  ``true``: the data source meets the XA specification.
   -  ``false``: the data source does not meet the XA specification.

-  **Fields by which the data is sorted in the source**
   (``DATAINORDERFIELDSLIST``). Configurable in all the wrappers except
   Custom ones. This property indicates the fields by which the data is
   ordered. The syntax of this property is the following: ``<field name> { ASC | DESC } [, <field name> { ASC | DESC }]*``
   
   E.g., let us say that we have a Web service base view with the fields
   ``F1`` and ``F2``. We know that the results of querying the Web
   service are ordered in ascendant order by the field ``F1`` and then,
   by descendant order by the field ``F2``. By setting this property to
   ``F1 ASC, F2 DESC``, we are telling Virtual DataPort the order
   of the result and it may be able to perform certain optimizations such
   as selecting a most efficient join algorithm.

-  **Delegate Operators List** (``DELEGATEOPERATORSLIST``). This property
   determines the list of operators that can be delegated to the data
   source. This list allows Virtual DataPort to optimize the query plan by
   delegating part of the processing to the source. While the Server
   carries out this action automatically on relational databases, other
   types of sources do not provide this information about their metadata.


**Example**: In the following example we create a DF wrapper indicating that the data of the file is
ordered by the column ``id`` (property ``DATAINORDERFIELDSLIST``). By
adding this property, a base view created over this wrapper can
participate in a ``MERGE JOIN``. Otherwise it cannot.



.. code-block:: vql
   :caption: DF Wrapper configuration example with SOURCECONFIGURATION
   :name: DF Wrapper configuration example with SOURCECONFIGURATION

   CREATE WRAPPER DF w_internet_inc
       DATASOURCENAME = df_ds_internet_inc
       OUTPUTSCHEMA (
           id (OPT),
           summary (OPT),
           ttime (OPT),
           taxid = 'taxId' (OPT),
           specific_field1 (OPT),
           specific_field2 (OPT)
       )
       SOURCECONFIGURATION (
       DATAINORDERFIELDSLIST = (id ASC)
   );

