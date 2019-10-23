==================
Path From Variable
==================

Use this path if the data will not be obtained from any source but it is
provided by clients in the ``WHERE`` clause of the queries that involve
the base views of the data source.

To use this type of path, you have to provide a name for the variable.
All the base views created over a data source with this type of path,
will have an extra field, which represents this variable.

Let us say that:

-  You create a DF data source with **Data route** **From Variable** and
   the name of the variable is “variable\_with\_data”.
   The **Column delimiter** is “,”, and select the **Header** check box.
-  Click **Create base view**. Now you will have to provide the value of
   the variable “variable\_with\_data” so the Server infers the schema
   of the data.
   For example:
   
   .. code-block:: none
   
      REGION,SALES,ACTIVITY
      NORTH,20000,75%
      SOUTH,51000,79%
      WEST,12000,72%

-  Then, every time a client queries this view, she will have to provide
   the value of the variable in the ``WHERE`` clause of the query. E.g.:
   
   .. code-block:: sql
   
      SELECT region, sales, activity 
      FROM bv_df_view_from_data    
      WHERE input_variable_with_data = 'REGION,SALES,ACTIVITY   
      NORTH,20000,75%
      SOUTH,51000,79%
      WEST,12000,72%';