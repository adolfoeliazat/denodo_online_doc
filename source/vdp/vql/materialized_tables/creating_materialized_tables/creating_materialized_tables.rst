============================
Creating Materialized Tables
============================

Syntax of the statement ``CREATE MATERIALIZED TABLE``.

.. code-block:: bnf
   :name:  Syntax of the statement CREATE MATERIALIZED TABLE
   :caption: Syntax of the statement CREATE MATERIALIZED TABLE
      
   CREATE [ OR REPLACE ] MATERIALIZED TABLE <name:identifier> 
     (
         <VDP type field> [, <VDP type field> ]* 
       | <SQL type field> [, <SQL type field> ]*
     )
     [ FOLDER = <literal> ]
     [ <primary key> ]
   
   CREATE [ OR REPLACE ] MATERIALIZED TABLE <name:identifier> 
     AS <SELECT statement>
   
   <VDP type field> ::= 
     <name:identifier> : <Virtual DataPort data type> 
       [ ( <property> [, <property> ]+ ) ]
   
   <SQL type field> ::= 
     <name:identifier> <SQL type> [ ( <property> [, <property> ]+ ) ]
   
   <SQL type> ::=
       BIT ( <integer> )
     | BIT VARYING ( <integer> )
     | BLOB
     | BOOLEAN
     | CHAR ( <integer> )
     | CHARACTER ( <integer> )
     | CHARACTER VARYING ( <integer> )
     | DATE
     | DECIMAL
     | DECIMAL ( <integer>, <integer> )
     | DOUBLE
     | DOUBLE PRECISION
     | FLOAT
     | INT
     | INTEGER
     | LONG
     | NCHAR ( <integer> )
     | NUMERIC
     | NUMERIC ( <integer> , <integer> )
     | NVARCHAR ( <integer> )
     | REAL
     | SMALLINT
     | TEXT
     | TIMESTAMP
     | TIMESTAMP WITH TIME ZONE
     | VARCHAR ( <integer> )
   
   <property> ::=
     <name:identifier> [ = <value:identifier> ]

   <primary key> ::= 
     [ CONSTRAINT <name:literal> ] 
     PRIMARY KEY ( <field name:literal> [, <field name:literal> ]* )

..

   <Virtual DataPort data type> ::= (see :ref:`Virtual DataPort data types`)
   

There are three ways of creating a materialized view:

.. rubric:: Syntax 1: CREATE MATERIALIZED TABLE

.. code-block:: vql
   :name: Example of CREATE MATERIALIZED TABLE
   :caption: Example of CREATE MATERIALIZED TABLE

   CREATE OR REPLACE MATERIALIZED TABLE usa_state (
            name : text        
          , abbreviation : text
          , capital : text
       )
   CONSTRAINT 'primary_key_usa_state' PRIMARY KEY ('name');
   ;
   
This statement creates a materialized table without data in it. The :ref:`next section<Inserting Data into Materialized Tables>` explains how to insert data in it.
   
The primary key field of this table is "name". Note that Virtual DataPort does not enforce the primary keys of views. You have to make sure you do not insert repeated values for this field.

|

.. rubric:: Syntax 2: CREATE MATERIALIZED TABLE ... AS SELECT ...

.. code-block:: bnf

   CREATE MATERIALIZED TABLE order_current_year 
   AS SELECT * 
   FROM order 
   WHERE TRUNC(date, 'YEAR') = TRUNC(NOW(), 'YEAR');
   
This statement creates a materialized view with the schema of the SELECT statement and inserts the result of the query in it.

|

.. rubric:: Syntax 3: SELECT ... INTO ...

.. code-block:: bnf
   :caption: Syntax of the statement SELECT ... INTO ...
   :name: Syntax of the statement SELECT ... INTO ...

   SELECT <expression> [, <expression>]* 
     INTO <table name:identifier> 
     FROM <FROM clause> 
     [ <WHERE clause> ] 
     [ <GROUP BY clause> ] 
     [ <HAVING clause> ]

This statement fails if the view <table name> already exists.

For example:
   
.. code-block:: bnf
   :caption: Example of SELECT... INTO
   :name: Example of SELECT... INTO
   
   SELECT order_date, total_amount
   INTO order_current_year
   FROM order 
   WHERE TRUNC(date, 'YEAR') = TRUNC(NOW(), 'YEAR');
   
This statement creates the materialized table "order_current_year" table with the fields "order_date" and "total_amount" and inserts the results of the query in it.

|

When creating a materialized table, the Server switches the database to
single user mode until the operation finishes. With the first syntax
(``CREATE MATERIALIZED TABLE ...``), the operation finishes instantly.

With the second syntax (``CREATE MATERIALIZED TABLE ... AS SELECT ...``)
and the third one (``SELECT ... INTO ...``), the database stays in single
user mode until the query finishes and its result is stored. If the
query is long, it is better to execute ``CREATE MATERIALIZED TABLE``,
which finishes instantly; and then, insert the data into the
materialized table with the command
``INSERT INTO <materialized table> ( <SELECT clause> )`` (see the
:ref:`next section <Inserting Data into Materialized Tables>`). As the INSERT does not switch the database to single user mode, inserting the data with the INSERT allows for a higher level of concurrency.