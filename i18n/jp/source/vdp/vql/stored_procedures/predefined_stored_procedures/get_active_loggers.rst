================================================
GET_ACTIVE_LOGGERS
================================================

.. rubric:: Description

The stored procedure ``GET_ACTIVE_LOGGERS`` returns information about the log categories that have been set in the Virtual DataPort server. It returns the categories set in the file :file:`{<DENODO_HOME>}/conf/vdp/log4j2.xml` and the ones set by the procedure :ref:`LOGCONTROLLER`.

.. rubric:: Syntax

.. code-block:: bnf

   GET_ACTIVE_LOGGERS()

-  The procedure does not have input parameters

|

The procedure returns:

-  ``log_category``: log category of the logger.
-  ``log_level``: log level of the logger. The possible values are ``TRACE``, ``DEBUG``, ``INFO``, ``WARN``, ``ERROR`` or ``FATAL``.

