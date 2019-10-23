======================
Creating Derived Views
======================

.. toctree::
   :hidden:

   creating_union_views/creating_union_views.rst
   creating_join_views/creating_join_views.rst
   creating_selection_views/creating_selection_views.rst
   creating_flatten_views/creating_flatten_views.rst
   creating_intersection_views/creating_intersection_views.rst
   creating_minus_views/creating_minus_views.rst
   creating_interface_views/creating_interface_views.rst
   viewing_the_schema_of_a_derived_view/viewing_the_schema_of_a_derived_view.rst
   tree_view/tree_view.rst
   data_lineage/data_lineage.rst
   advanced_configuration_of_views/advanced_configuration_of_views.rst
   materialized_tables/materialized_tables.rst
   temporary_tables/temporary_tables.rst
   querying_views/querying_views.rst
   editing_replacing_a_view/editing_replacing_a_view.rst

This section describes how to create derived views based on the base views that retrieve data from different sources.

The following sections describe the process of creating the following
types of views using our example to illustrate the process:

-  Union views: see section :ref:`Creating Union Views`
-  Join views: see section :ref:`Creating Join Views`
-  Selection views: see section :ref:`Creating Selection Views`
-  Flatten views: see section :ref:`Creating Flatten Views`
-  Intersect views: see section :ref:`Creating Intersection Views`
-  Minus views: see section :ref:`Creating Minus Views`
-  Interface views: see section :ref:`Creating Interface Views`


We will use the following example as a guide when describing the
process:

**Example**: Unified data about customer sales and incidents.

A telecommunications company offers phone and internet services to its
clients. Data on the incidents reported in the phone service are stored
in a relational database, which is accessed through JDBC. In addition,
data on the incidents reported in the Internet service are stored in
another relational database also accessed through JDBC.

In our example, the director of the I.T department wants to monitor the
number of incidents (either telephony or Internet) notified by the
clients with the greatest sales volume to establish whether measures
should be taken to increase client satisfaction.

Data on customer sales volumes are managed by another department of the
company. That department provides a Web Service so the other departments
can access to that data.

In this example, we will see how Virtual DataPort can be used to build a
unified data view to meet the needs of the I.T department, by obtaining
the total number of incidents from clients with the greatest sales
volumes.

The path :file:`{<DENODO_HOME>}/samples/vdp/incidents` contains:


-  SQL scripts for creating the tables used in the examples of this
   manual (version for the MySQL, Oracle and PostgreSQL databases).
   
   To follow the examples of this guide, use one of these
   scripts to create the required tables in a database.


-  A ``.war`` file with the implementation of the Web Service used in the
   examples. It has been tested with `Apache Tomcat 8.x <http://tomcat.apache.org/>`_
   and `Apache Axis 1.x <http://axis.apache.org/axis/>`_.


-  A WSDL file with the description of the Web service used


-  VQL scripts (VQL stands for Virtual Query Language) that create the
   objects (data sources, views, stored procedures...) that we will learn to
   create in this manual. You do not need to use them if you are going to
   follow this guide. Otherwise, edit the VQL script that matches your
   database:

   -  Replace ``@HOSTNAME`` with the host name of your database.
   -  In the ``CREATE WRAPPER JDBC`` statements, change the parameter
      ``RELATIONNAME``, so it matches the name of the schema in your
      database. E.g. change ``RELATIONNAME='VDB.INTERNET_INC'`` for
      ``RELATIONNAME='test.INTERNET_INC'``.
