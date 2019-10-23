=====================
Connection Parameters
=====================

The statements to create SOAP Web services, REST Web services and old
Web services have the same parameters to manage the connection
established by the Service to Virtual DataPort, in order to retrieve the
data from the published views.

These parameters are the following:

-  ``CHUNKSIZE``, ``CHUNKTIMEOUT``, ``QUERYTIMEOUT``. Their
   interpretation is the same as in any other Virtual DataPort client
   (see the section :doc:`Connection Parameters </vdp/administration/installation_and_execution/launching_the_virtual_dataport_administration_tool/tool_preferences>` of the Administration
   Guide).
-  ``POOLENABLED``. If ``TRUE``, the Service will use a pool of
   connections to the Server. We strongly recommend setting this to
   ``TRUE``.
-  ``POOLINITSIZE``. If ``POOLENABLED`` is ``TRUE``, it is the initial
   number of connections to be opened in the pool.
-  ``POOLMAXACTIVE``. If ``POOLENABLED`` is ``TRUE``, it is the maximum
   number of connections in the pool. If it is a negative value, the
   number is unlimited.
