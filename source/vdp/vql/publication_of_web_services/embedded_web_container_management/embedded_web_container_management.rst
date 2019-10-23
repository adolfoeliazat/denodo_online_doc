=================================
Embedded Web Container Management
=================================

You can manage the Web container embedded into the Denodo Platform with
the ``WEBCONTAINER`` statement.



.. code-block:: bnf
   :caption: Syntax of the WEBCONTAINER statement
   :name: Syntax of the WEBCONTAINER statement

   WEBCONTAINER {
         START
       | STOP
       | STATUS
       | SET <propertylist> }


This statement has the following parameters:

-  ``START`` / ``STOP`` / ``STATUS``: parameters to start, stop and
   check the current status of the Web container.
-  ``SET``: specifies the port numbers used by the embedded Web
   container.

The command ``DESC WEBSERVICE <web service name:literal>`` provides a
more detailed description of a Web service: operations, fields of every
operation, etc.

