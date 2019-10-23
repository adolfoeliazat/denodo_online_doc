=============
SELECT Clause
=============

The ``SELECT`` clause indicates the attributes to be obtained from the
relations specified in the ``FROM`` clause.

If the character ``*`` is specified in the ``SELECT`` clause, the
Server will project all the attributes of the queried views. In this
case, the order of the columns in the result will be the order in which
they appear in the ``CREATE VIEW`` or ``CREATE TABLE`` statement used to
create the view.

Aliases may also be defined for the columns obtained, thus allowing the
name of any attribute to be modified.

In the case of derived attributes (see section :ref:`Derived Attributes`),
if an alias is not specified, Virtual DataPort will automatically
generate a name for the new attribute.

In the queries and views, no two fields with the same name are allowed,
so it would be necessary to rename any of them (by using aliases).

The ``DISTINCT`` modifier may be included. In this case, all
duplicated tuples will be deleted from the result.

You can project simple boolean expressions like 

:: 
    
   field = 'value'
    
Or 

:: 
     
   field_1 = field_2

Derived Attributes
==================

The ``SELECT`` clause may include derived attributes. These attributes
are created by evaluating an expression that may use functions,
constants and the values of other attributes.

A description of the functions supported by Virtual DataPort can be
found in the section :ref:`Functions for Conditions and Derived attributes`.
Detailed examples of the use of each function can be found in the section :ref:`Syntax of Condition Functions`.

Some examples of how to use derived attribute functions are shown below.
The following query obtains a column named ``newSalary`` containing the
result of adding ``1000`` to the values contained in the ``salary``
column of the ``emp`` view.

.. code-block:: sql

   SELECT SUM(1000, salary) newSalary
   FROM emp;

And the following example shows how to use a nested function as
parameter:

.. code-block:: sql

   SELECT NAME, SUM(SALARY, DIV(SALARY,1000)) salaries
   FROM emp;
