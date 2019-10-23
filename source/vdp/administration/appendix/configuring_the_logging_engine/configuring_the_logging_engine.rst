==============================
Configuring the Logging Engine
==============================

Virtual DataPort uses the logging library `Apache Log4j 2 <http://logging.apache.org/log4j>`_
to log the activity of the
Virtual DataPort server and `Apache Log4j 1.x <http://logging.apache.org/log4j/1.2/>`_
to log the activity of the other components. Log4j allows
defining several categories on which the log level will be specified
independently. For each category, the log level can be ``TRACE``,
``DEBUG``, ``INFO``, ``WARN``, ``ERROR`` or ``FATAL``.

The Virtual DataPort Server logs are stored in
:file:`{<DENODO_HOME>}/logs/vdp` and the Administration Tool logs, in
:file:`{<DENODO_HOME>}/logs/vdp-admin`.

The configuration of Log4j of the Virtual DataPort server is controlled
by the file :file:`{<DENODO_HOME>}/conf/vdp/log4j2.xml`; for the
administration tool by :file:`{<DENODO_HOME>}/conf/vdp-admin/log4j.xml`.

You need some knowledge of Log4j to modify these configuration files.
Their syntax is different because the Server uses Log4j version 2 and
the other components, Log4J version 1.

To apply the changes made on ``log4j2.xml`` or ``log4j.xml``, you have
to restart the Server or the administration tool respectively.

You can change the log level of a category in the Server without
restarting by doing the following:

#. Invoking the stored procedure ``LogController``. See more about this
   in the section :ref:`LogController <LOGCONTROLLER>` of the VQL Guide.
#. Or, invoking the operation ``setLogLevel`` of the
   ``LogManagementInfo`` MBean. See more about this operation in the
   section :ref:`LogManagementInfo MBean`.

The changes in the log levels performed at runtime are lost when the
Server is restarted.

.. note:: In production environments, we recommend using an ``ERROR``
   *log* level. Lower levels degrade the system performance and are only
   recommended for debugging tasks.
