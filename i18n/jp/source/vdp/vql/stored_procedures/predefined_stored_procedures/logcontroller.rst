=============
LOGCONTROLLER
=============

.. rubric:: Description

The stored procedure ``LOGCONTROLLER`` changes the log level for a
certain log category of the logging engine used by Virtual DataPort.

The changes performed by this procedure are lost when the Server is
stopped.

Read more about the logging engine in the section :ref:`Configuring the
Logging Engine` of the Administration Guide.

.. rubric:: Syntax

.. code-block:: bnf

   LogController ( 
          category : text
        , <level>
   )
   
   <level> ::=
         'TRACE'
       | 'DEBUG'
       | 'INFO'
       | 'WARN'
       | 'ERROR'
       | 'FATAL'

-  ``category``: the category of the log. You can see the list of log
   categories in the :file:`{<DENODO_HOME>}/conf/vdp/log4j.xml` file.
-  ``level``: the type of messages that you want to log for this
   category.

.. rubric:: Privileges Required

Only administrators can invoke this procedure.

.. rubric:: Example

.. code-block:: vql

   CALL LOGCONTROLLER('com.denodo.vdp.requests', 'INFO')

With the default Log4j configuration of the Virtual DataPort server,
after executing this query, all the requests sent to the Server will be
logged in the :file:`{<DENODO_HOME>}/logs/vdp/vdp-requests.log` file.
