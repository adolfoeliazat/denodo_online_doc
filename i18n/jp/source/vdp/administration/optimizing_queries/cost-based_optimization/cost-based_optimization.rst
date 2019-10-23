=======================
Cost-Based Optimization
=======================

.. toctree::
   :hidden:
   
   enabling_the_cost-based_optimization.rst
   gathering_the_statistics_of_views.rst
   tuning_the_cost-based_optimization_process.rst
   current_limitations_of_the_cost-based_optimization_process.rst
   

The execution engine, after applying the static optimizations described
in the previous section, selects the best execution plan based on the
estimated cost of executing them.

To estimate the cost of an execution plan, it uses:

-  When the data is obtained from a database, the indexes of the queried
   tables in the database.
-  Statistics about the views’ data: number of rows, number of NULL
   values of each field, etc.
   
   To obtain the statistics of JDBC base views, Virtual DataPort can query
   the system tables of the database. Alternatively, it can also query the
   view with a SELECT statement to obtain these statistics

In this section, we describe the cost-based optimization support and
provide some tips to use it effectively:

-  The section :ref:`Enabling the Cost-Based Optimization` describes how to
   enable the cost-based optimization.
-  The section :ref:`Gathering the Statistics of Views` describes how to
   obtain the statistics required by the cost-based optimization
   process.
-  The section :ref:`Tuning the Cost-Based Optimization Process` describes
   the information used to generate cost estimations and provides some
   tuning tips for the process.
-  The section :ref:`Current Limitations of the Cost-Based Optimization
   Process` describes some limitations of this optimization.

