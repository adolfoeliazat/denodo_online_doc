===================================================
Mapping Multidimensional Data to a Relational Model
===================================================

.. toctree::
   :hidden:
   
   creating_a_multidimensional_base_views_over_a_multidimensional_data_source.rst
   mapping_the_result_of_mdx_queries.rst

Virtual DataPort can obtain data from several multidimensional databases
(SAP BW 3, SAP BI 7, Mondrian, Oracle Essbase, etc.)

This section explains how the data of a multidimensional structure is
mapped to a relational representation.

The members of a multidimensional *cube* are organized in *dimensions*,
*hierarchies* and *levels*:

-  Each dimension may have one or more hierarchies
-  Each hierarchy is composed by a set of levels
-  Levels have a hierarchical organization. That is, a level may have a
   parent and/or a children level.

To extract data from a multidimensional database, you have to create a
multidimensional base view. There are two ways to do this:

#. Graphically, by selecting the hierarchies of the dimensions,
   measures, attributes and variables, that will form the base view. See
   section :ref:`Creating a Multidimensional Base Views over a
   Multidimensional Data Source`.
#. Or, writing an MDX query.

