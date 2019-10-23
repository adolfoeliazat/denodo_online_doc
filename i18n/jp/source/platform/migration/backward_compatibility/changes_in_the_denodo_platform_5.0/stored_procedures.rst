=================
Stored Procedures
=================

Starting with Denodo 5.0, the methods
``getXXX(<parameter name:String>)`` of the ``ResultSet`` objects (i.e.
``getString(...)``, ``getInt(...)``, etc.) are case sensitive. In the
4.7 version, they were not.

See the following example:

.. code-block:: java

   String query = "select '1' as field1 from dual()";
   ResultSet resultSet = getEnvironment().executeQuery(query);
   if (!resultSet.next()) {
       throw new StoredProcedureException("not found any row");
   }

   /*
    * The following fails in Denodo 5.0 and newer versions because the 
    * "getXXX" methods of the "ResultSet" class (i.e. getString(...), 
    * getInt(...), etc.) are case sensitive. That means, that the input 
    * parameter has to have the same case than the name of the field in 
    * the query  (i.e. "test").
    * This worked in 4.7.
    */
   String text1 = resultSet.getString("Field1");

   /*
    * The following works in all versions because the parameter of 
    * the "getString(...)" method has the same case as the name of the 
    * field in the result set.
    */
   String text2 = resultSet.getString("field1");

