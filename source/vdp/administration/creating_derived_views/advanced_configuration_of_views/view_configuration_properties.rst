=============================
View Configuration Properties
=============================

The View Configuration Properties, also called *wrapper source
configuration*, allow indicating specific characteristics of the
underlying data sources such as their distributed transaction support
capacity or whether insert operations are allowed.

This section lists the configurable properties of a base view, depending
on the type of data source they have come from.

The section :doc:`/vdp/administration/creating_data_sources_and_base_views/data_source_configuration_properties/data_source_configuration_properties` lists the
configuration properties of the data sources.

.. note:: Usually, users do not need to edit these properties since
   Virtual DataPort automatically uses suitable configurations for most
   common data sources.

To configure the properties of a base view, open the **Options** dialog
of the view and then, click on the **Search methods** tab. In this
dialog, underneath the search methods (using the scroll bar where
necessary), click **Wrapper Source Configuration**. The configurable
properties are the following:


-  **Allow insert** (configurable in JDBC and ODBC base views): If *yes*,
   it indicates that the underlying data source supports ``INSERT``
   operations.
   If *no*, the ``INSERT`` operations over this view will be rejected.


-  **Allow delete** (configurable in JDBC and ODBC base views): If *yes*,
   it indicates that the underlying data source supports ``DELETE``
   operations.
   If *no*, the ``DELETE`` operations over this view will be rejected.


-  **Allow update** (configurable in JDBC and ODBC base views): If *yes*,
   it indicates that the underlying data source supports ``UPDATE``
   operations.
   If *no*, the ``UPDATE`` operations over this view will be rejected.


-  **Delegate SQL sentence as subquery** (configurable in JDBC base
   views): If *yes* (default value) and the base view has been created
   from a SQL query, at runtime, Virtual DataPort will be able to
   delegate the SQL query of the base view as a subquery in the ``FROM``
   clause. This increases the number of operations that can be delegated
   to the database.
   
   For example, let us say that we have created a JDBC base view called
   ``customer_info`` from the following SQL query:
   
   .. code-block:: sql
   
      SELECT id, name 
      FROM customer 
      WHERE id = function_not_supported_by_vdp(name, address)
   
   Then, a user executes the following query in Virtual DataPort:
   
   .. code-block:: sql
   
      SELECT COUNT(*) 
      FROM customer_info 
      GROUP BY name
   
   If this property is set to *no*, Virtual DataPort executes the SQL
   query of the view ``customer_info`` in the database and then, over the
   results obtained from the database, it executes the ``GROUP BY``
   operation and the ``COUNT`` function.
   
   If this property is set to *yes*, the entire query can be delegated to
   the database by putting the SQL query of the base view in a subquery:
   
   .. code-block:: sql
   
      SELECT COUNT(*) FROM (
          SELECT id, name 
          FROM customer 
          WHERE id = function_not_supported_by_vdp( name, address ) 
      ) GROUP BY name

   In this scenario, Virtual DataPort does not have to process the
   ``GROUP BY`` and the ``COUNT`` and the amount of traffic between the
   database and Virtual DataPort is diminished.

   Although this property is set to “yes”, the entire query cannot always
   be executed in the database. This happens in the following scenarios:


   -  The query executed in Virtual DataPort involves functions not supported
      by the database.

   -  A SQL statement is *not* delegated as a subquery if it contains the
      token ``@WHEREEXPRESSION``.

   -  A nested join cannot be delegated to the database if the following
      conditions are met:

      -  The base view created from a SQL query, is used in the right side of
         the join.
      -  The SQL query of the base view has interpolation variables.
      -  And, the values of these variables are obtained from the query of the
         left side of the join.


   .. note:: Set this property to *no* if the SQL query cannot be delegated
      as a subquery. For example, if the query uses “common table
      expressions”. E.g.

      .. code-block:: sql
      
         WITH cte AS (SELECT * FROM VIEW)
         SELECT * FROM cte


-  **Supports distributed transactions** (configurable in JDBC and ODBC
   base views): If *yes*, it indicates that the underlying data source can
   take part in XA distributed transactions.


-  **Fields by which the data is sorted in the source** (configurable in
   all the base views, except Custom ones): This property indicates the
   fields by which the data is ordered. The syntax of this property is
   the following:
   ``<field name> { ASC | DESC } [, <field name> { ASC | DESC }]*``

   For example,
   ``field 1 DESC``

   E.g., let us say that we have a Web service base view with the fields
   ``F1`` and ``F2``. We know that the results of querying the Web service
   are ordered in ascendant order by the field ``F1`` and then, by
   descendant order by the field ``F2``. By setting this property to
   ``F1 ASC, F2 DESC``, we are telling Virtual DataPort the order of the
   result and it may be able to perform certain optimizations such as
   selecting a most efficient join algorithm. See more about this in the
   section :ref:`Merge Join`.


-  **Delegate operators list** (configurable in Multidimensional base views
   that connect to SAP and Web service base views): This property
   determines the list of operators that can be delegated to the data
   source. This list allows Virtual DataPort to optimize the query plan by
   delegating part of the processing to the source. While the Server
   carries out this action automatically on relational databases, other
   types of sources do not provide this information about their metadata.


Configuration Properties for Specific View Types
=================================================================================

The Google Search (see section :ref:`Google Search Sources`) data sources have specific
configuration properties.

To configure the properties of these base views, open the base view by
double-clicking on it in the Server Explorer and then click **Options**.
In this dialog, underneath the search methods (using the scroll bar
where necessary), click on **Wrapper Source Configuration** to open the
property configuration screen.

The configuration properties of Google base views are “Site
Collections”, “Client”, “Languages” and “Number of Key match”. See
section :ref:`Google Search Sources` for more information about these
properties.
