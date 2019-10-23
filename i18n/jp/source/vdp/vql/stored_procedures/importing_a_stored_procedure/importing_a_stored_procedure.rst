============================
Importing a Stored Procedure
============================

The statement ``CREATE PROCEDURE`` adds a new stored procedure
to the Virtual DataPort Server.

.. code-block:: bnf
   :caption: Syntax of the CREATE PROCEDURE statement
   :name: Syntax of the CREATE PROCEDURE statement

   CREATE [OR REPLACE] PROCEDURE <name:identifier>
       CLASSNAME <className:literal>
       [ CLASSPATH <classPath:literal> ]
       [ JARS <jar name:literal> [, <jar name:literal>]* ]
       [ FOLDER = <literal> ]
       [ DESCRIPTION = <literal> ]


``CLASSNAME``: name of the Java class that
implements the stored procedure. This class must be loaded into the
Server (see the section :ref:`Importing Extensions` of the Administration
Guide).

``CLASSPATH``: list of paths to the jar files that contains the Java classes of the procedure.
Although you can use this parameter, we recommend importing the jar files into the Virtual DataPort server and then,
reference them from the ``JARS`` clause. That way, you do not depend on these jars being on a particular location.

``JARS``: list of names of the jar files that have been uploaded to the Virtual DataPort server and that this procedure depends on.

The use of the ``OR REPLACE`` modifier specifies that, if there is a
procedure with the name indicated, this must be replaced by the new
procedure. This will lead to the recalculation of the schemas and query
capabilities of the derived views using the procedure.

Once created, a stored procedure can be modified using the
``ALTER PROCEDURE`` statement.


.. code-block:: bnf
   :caption: Syntax of the ALTER PROCEDURE statement
   :name: Syntax of the ALTER PROCEDURE statement

   ALTER PROCEDURE <name:identifier>
       [ CLASSNAME <className:literal> ]
       [ CLASSPATH = <classPath:literal> [<classPath:literal> ]* ]
       [ JARS <jar name:literal> [, <jar name:literal>]* ]


The meaning of the ``CLASSNAME`` and ``CLASSPATH`` clauses is the same
for the ``CREATE PROCEDURE`` statement.

