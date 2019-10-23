===============================================
Query Capabilities: Search Methods and Wrappers
===============================================


.. toctree::
   :hidden:

   query_constraints.rst
   assigning_wrappers_to_search_methods.rst
   example_of_how_a_search_method_is_created.rst

Some sources offer limited query capabilities. For instance, most web
sources can only be queried with constraints imposed by a specific HTML
query form.

The description of the query capabilities in Virtual DataPort is done
through the so-called *search methods*. For each view, the administrator
can define one or more search methods.

When creating a search method, the following elements should be
specified: the list of query constraints, the list of output attributes
and the *wrapper*, created beforehand using the statement
``CREATE WRAPPER``, which is responsible for extracting the data from
the source.
