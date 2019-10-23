=======================
Defining JAR Extensions
=======================

Stored procedures, Custom functions and Custom wrappers are developed
with Java.

The section :doc:`/vdp/developer/developing_extensions/developing_extensions` of the Developer Guide explains how
to develop them.

.. note:: We strongly recommend using the Administration Tool to load
   extensions (see the section :ref:`Importing Extensions` of the Administration
   Guide).

The ``CREATE JAR`` statement adds these new Java libraries (jar
files) to the Server.


.. code-block:: bnf
   :caption: Syntax of the CREATE JAR statement
   :name: Syntax of the CREATE JAR statement

   CREATE [ OR REPLACE ] JAR <name:identifier> <jar encoded as base64:literal>


An identifier must be specified for the ``jar`` file along with its
contents coded as a string of bytes. The ``OR REPLACE`` modifier
replaces the file, if it already exists.

Virtual DataPort establishes a dependency between the jar files imported
into the Server and the Denodo stored procedures and Custom Wrappers
that use the Java classes provided by these jars.

If a user executes the statement ``DROP JAR`` to delete a jar and this
jar has a dependency, the statement will fail. In this case, you can
execute ``DROP JAR <name> CASCADE`` to delete the jar file respectively.
This statement deletes the jar file *and* the elements that depend on
them.

