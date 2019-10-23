===================
Using the VQL Shell
===================

With the VQL Shell tool you can execute any VQL statement, including queries and DDL statements (``CREATE VIEW``, ``DROP VIEW``, etc.). To execute several statements at once, separate them with ``;``. You can enter them or load them from a file with the button “Load”.

The VQL Shell has several controls, many of them self-explanatory. This list contains the ones that need some clarification:

-  **Execute**. Executes all the queries of the box. To execute only part of what you entered in the queries box, select that part and press **Ctrl+Enter**.

-  **Load**. Loads the content of a file into the commands box. To load a
   VQL file that creates several databases, do it from the “Import” wizard on the File menu instead of from the VQL Shell. The reason is that the “Import” wizard is capable of importing correctly elements of a database that depend on elements of another database.

-  **Display rows**. The Tool only displays the first N rows of the result.

-  **Retrieve all rows**. If selected, the Tool does not display more rows than *Display rows*, but it process the entire result set. 
   If cleared, the Tool adds the clause ``LIMIT`` to the query behind the scenes. E.g. if *Display rows* is set to 100 and you execute the query ``SELECT * FROM sales``, the Tool sends the query ``SELECT * FROM sales LIMIT 100`` so the query will finish after returning 100 rows.

   Selecting this check box is useful to:

   -  Estimate how much time it will take for a client application to execute this query.
   -  If you are loading the cache of a view from the *Execute* dialog or the VQL Shell, you have to select this check box. Otherwise, the cache will not be loaded correctly (see the explanation below).
   

   .. important:: Clearing the check box *Retrieve all rows* affects the queries that load the cache of a view because the query is interrupted before it finishes:
   
      -  When querying a view with cache mode Partial, the cache of the query will not be loaded.
      -  When querying a view with cache mode Full and you select *Store results in cache* (``'cache_preload'='true'``), the maximum
         number of rows stored in the cache is the value of *Display rows*.
      -  When executing the command ``CREATE REMOTE TABLE``, the maximum number of rows inserted in the remote table is 
         the value of *Display rows*.
         
      If you select *Retrieve all rows*, the queries that load the cache will work as expected.

-  Press **Ctrl+F** to open a find and replace dialog.

|

When executing a query, the Tool will show the **Query Results** tab. In this tab you can do the following:

-  Click **Save** to store the result of the query to a CSV file. Note that this only saves the data that the administration tool is showing. For example, if *Display rows* is 150 but the query returns 200 rows, the Tool will only save these 150 rows.
-  See the **Execution Trace** of the query.   The section :ref:`Execution Trace of a Statement` explains the elements displayed in the execution trace of a query.

|

The VQL Shell includes a “command log” that saves the last VQL statements run. This command log is maintained between several executions of the Administration Tool. You can see it in the tab **Execution Log**.

|

You can use the Server Explorer to speed up writing VQL statements:

-  Drag an element to add its name to the current VQL statement.
-  Right-click on a view and click **VQL Shell** > **Select...** to add the statement ``SELECT <fields of the view> FROM <view name>``.
-  Right-click on any element and click **VQL Shell** > **Desc...** to add the statement ``DESC VQL <element type> <element name>``.
