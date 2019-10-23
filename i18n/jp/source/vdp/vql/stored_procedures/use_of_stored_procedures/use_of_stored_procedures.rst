========================
Use of Stored Procedures
========================

There are several ways of invoking a Denodo stored procedure:

1. Invoking the procedure on the FROM clause of a SELECT statement and providing the input parameters on the 
   WHERE clause. For example:

   .. code-block:: vql

      SELECT database_name, name, type, description, view_type, swap_active, folder 
      FROM GET_VIEWS()
      WHERE 
          input_database_name = 'customer360' 
      AND (input_init_create_date = ADDDAY(CURRENT_DATE, -1));

   When a stored procedure is invoked this way, the schema of the result includes the output parameters of the stored procedure 
   *and its input parameters*. This allows to invoke the stored procedure in the same way as a view and join the 
   result of two stored procedures.
   
2. With the statement ``CALL``: 

   .. code-block:: bnf
      :name: Syntax of the CALL statement 
      :caption: Syntax of the CALL statement 
   
      CALL <procedureName:identifier> 
              ( [ <paramValue:literal> [ ,<paramValue:literal> ]* ] )
      [ CONTEXT ( 'i18n' = <literal> ) ] [ TRACE ]

   The following statement invokes the stored procedure ``GET_VIEWS``, with the parameter ``input_database_name`` = "customer360":
   
   .. code-block:: sql
   
      CALL GET_VIEWS( 'customer360', null, null, null, null, null, null, null, null, null, null, null);
   
   If an input parameter of a stored procedure is optional, pass ``NULL``.

3. Invoking the procedure on the FROM clause of a SELECT statement. For example:

   .. code-block:: vql
   
      SELECT avgrevenue  
      FROM CalculateAvgRevenue( {ROW('B78596011'), ROW('B78596012')} )

   In this example, the input parameter of the ``CalculateAvgRevenue`` procedure is an array of registers. Each register contains a single field, which is a client’s Tax ID.

|

We recommend invoking stored procedures the first way because:

i. The query contains the names of the input parameters so it is easy to understand 
   what the query does. With ``CALL`` you have to know the meaning of each parameter.
#. You do not need to provide a value for all the parameters. 
#. Unlike with CALL, invoking the procedure from a SELECT statement, allows you to join the result of the procedure with other views.
