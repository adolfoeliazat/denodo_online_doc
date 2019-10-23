=======================================
Inserting Data into Materialized Tables
=======================================

There are two ways of inserting data into a materialized view:

.. rubric:: Syntax 1

.. code-block:: bnf

   INSERT INTO <materialized table> ( <fields> ) 
   VALUES (<values> ) [, ( <values> ) ]*

Inserts one or more rows into a materialized table.   

This statement supports the same options as the one available for base and derived views
(see :ref:`Syntax of the INSERT statement`) with the addition than you can insert several 
rows within the same statement. For example:

.. code-block:: sql
   :name: Example of inserting several rows in materialized table using a single INSERT statement
   :caption: Example of inserting several rows in materialized table using a single INSERT statement

   INSERT INTO usa_state (name, abbreviation, capital) VALUES 
     ('Arizona', 'AZ', 'Phoenix')
   , ('California', 'CA', 'Sacramento')
   , ('New York', 'NY', 'Albany')
   , ('North Dakota', 'ND', 'Bismarck');

|

.. rubric:: Syntax 2


.. code-block:: bnf
      
   INSERT INTO <materialized table> ( <SELECT clause> ) 

Inserts the result of a ``SELECT`` clause into a materialized table. The benefit
over the previous syntax is that with this one, you can insert
several rows at once.

The result of the query has to have the same schema as the
materialized table and in the result, the fields must be in the same
order as in the target table.

.. code-block:: sql
   :name: Example of the statement INSERT... INTO
   :caption: Example of the statement ``INSERT... INTO``

   INSERT INTO usa_state (
     SELECT 'Arizona', 'AZ', 'Phoenix'
   );
