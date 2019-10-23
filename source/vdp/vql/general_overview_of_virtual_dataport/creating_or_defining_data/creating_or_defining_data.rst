=========================
Creating or Defining Data
=========================

This section is a general overview of how to define views or relations,
which constitute the global schema of Virtual DataPort. All the tasks
involved in the creation and administration of schemas will be described
in detail in subsequent sections of this document.

To retrieve data from the different types of data sources, you need to:

#. Define a *data source*, which holds information about how to connect
   to a certain source: a relational database, a Web service, etc.
#. A *wrapper*, which extracts data from an element of a data source.
   For example, they can retrieve data from a table of a database or
   invoke an operation of a Web service.
#. A *base view*, which represent the data obtained by the *wrapper*.
#. After defining one or more base views, they can be combined, creating
   more views. Then, users can query these new views, obtaining data
   which are a combination of data obtained from different sources.

The following subsections describe these operations:

.. toctree::
   :maxdepth: 1

   defining_base_views.rst
   defining_data_sources_and_wrappers.rst
   defining_the_views_of_the_global_schema.rst
