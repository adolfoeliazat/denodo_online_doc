=========================
Creating Temporary Tables
=========================

The following table contains the syntax to create temporary tables:



.. code-block:: bnf
   :caption: Syntax of the statement CREATE TEMPORARY TABLE
   :name: Syntax of the statement CREATE TEMPORARY TABLE

   CREATE TEMPORARY TABLE <name:identifier>
     ( 
         <VDP type field> [, <VDP type field> ]* 
       | <SQL type field> [, <SQL type field> ]* 
     )
     [ FOLDER = <literal> ]
     [ <primary key> ]

   <VDP type field> ::= 
       <name:identifier> : <Virtual DataPort data type> 
           [ ( <property list> ) ]
   
   <Virtual DataPort data type> ::= (see Figure 1)
   
   <SQL type field> ::= <name:identifier> <SQL type>
           [ ( <property> [, <property> ]+ ) ]
   
   <SQL type> ::=
       | BIT ( <integer> )
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
   
   CREATE TEMPORARY TABLE <name:identifier> 
       [ ( <field without type> [, <field without type> ]* ) ]
       AS <select>
   
   <field without type> ::= <name:identifier> [ ( <property list> ) ]
   
   <property list> ::=
       <property name:identifier> [ = <value:identifier> ]
         [, <property name i:identifier> [ = <value i:identifier> ] ]*

   <primary key> ::= 
     [ CONSTRAINT <name:literal> ] 
     PRIMARY KEY ( <field name:literal> [, <field name:literal> ]* )
   
..

   <select> ::= (see :ref:`Syntax of the SELECT statement`)


To indicate the type of the fields you can either use the native VQL
data types or the SQL data types. The SQL data types are transformed
into

For example:



.. code-block:: vql
   :caption: Example of CREATE TEMPORARY TABLE
   :name: Example of CREATE TEMPORARY TABLE

   CREATE TEMPORARY TABLE employee (
         ssn : text
       , first_name : text
       , surname : text
   )
   CONSTRAINT 'primary_key_employee' PRIMARY KEY ('ssn');

This creates a temporary table with the field "ssn" as primary key. Note that Virtual DataPort does not enforce the primary keys of views. You have to make sure you do not insert repeated values for this field.


.. code-block:: vql
   :caption: Example of creating a temporary table from the result of a query
   :name: Example of creating a temporary table from the result of a query

   CREATE TEMPORARY TABLE employee_dept_10 AS SELECT * FROM employee
   WHERE dept_no = 10;


After creating a temporary table, you can insert data into it as with a
regular view and also insert several rows in the same statement.

For example,



.. code-block:: sql
   :caption: Examples of how to inserting data in a temporary table
   :name: Examples of how to inserting data in a temporary table

   INSERT INTO employee (first_name, surname, ssn)
   VALUES ('Emma', 'Smith', '987-65-4321');

   INSERT INTO employee (SELECT 'Robert', 'Brown', '123-45-6789' from
   Dual());

   INSERT INTO employee (first_name, surname, ssn) VALUES
   ('Emma', 'Smith', '987-65-4321')
   , ('Elizabeth', 'Brown', '987-65-4322');


See more details about the ``INSERT`` clause in the section :ref:`INSERT
Statement`.