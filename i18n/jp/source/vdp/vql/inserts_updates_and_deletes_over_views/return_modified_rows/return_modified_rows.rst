====================
Return Modified Rows
====================

At times it is useful to obtain the values of the rows you insert or modify on a database.
The INSERT and UPDATE statements support the RETURNING clause, which allows you to do this on a single statement. With this clause you avoid having to execute a query after an INSERT or an UPDATE to obtain the values of the rows that have just been inserted or modified by the INSERT/UPDATE.

In the RETURNING clause you indicate the name of the fields for which you want to obtain the value.

Adding the RETURNING to an INSERT is useful to obtain these types of values:

-  The value of fields that are auto-incremental. That is, fields of new rows for which the database automatically assigns an identifier.
-  The value of fields that are auto-generated. That is, fields for which the database assigns a value that is calculated based on the value of other fields of the same row.
-  The value of fields with a default value. That is, fields to which the database automatically assigns a value, if the INSERT does not provide one.

For example:

.. code-block:: sql
   :emphasize-lines: 2

   INSERT INTO employee (name, email, deparment_id) VALUES ('Jackson, Peter', 'pjackson@acme.com', 10)
   RETURNING salary;

This INSERT returns the value of the field "salary" of the employee.

This example assumes that the database automatically assigns a value to the field "salary" to the new employees, when the INSERT does not provide one.

|

In an UPDATE, the data available to RETURNING is the new content of the modified row. For example:

.. code-block:: sql
   :emphasize-lines: 3

   UPDATE employee SET salary = salary * 1.03
   WHERE department = 'hr'
   RETURNING salary, bonus;

This statement modifies the value of the column salary and returns the bonus and the new salary of the employees of the department of human resources.

Limitations
===========

-  This feature is only available for views whose data comes from a JDBC database; not for other types of sources.

-  This feature relies on the underlying database to return the requested fields. Although almost all the databases support RETURNING, many have limitations on what fields you can pass to the RETURNING clause. For example, many databases ignore the RETURNING clause on UPDATE statements when the UPDATE modifies more than one row. Other databases only support returning the value of the primary key, etc.
   
-  The execution engine of Denodo does not check these limitations; it only passes to the database the fields in the RETURNING clause. The underlying database may return an error if the INSERT/UPDATE tries to obtain values that the database cannot return.