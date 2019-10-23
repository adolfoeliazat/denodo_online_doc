==============================
Enabling Wrapper Trace Logging
==============================

In order to enable wrapper execution trace logging, the configuration
parameter ITPCore.LOGGING\_MODE must be set in the appropriate
application configuration file. This configuration file is
<DENODO\_HOME>/conf/vdp/VDBConfiguration.properties for the Wrapper
Server and
<DENODO\_HOME>/conf/itp-admin-tool/ITPAdminConfiguration.properties for
the Wrapper Generation Tool. The parameter accepts the following values:



-  ITPCore.LOGGING\_MODE=0
  
   Wrapper execution trace logging disabled. This is the default value.

-  ITPCore.LOGGING\_MODE=1
 
   Wrapper execution trace logging enabled. The traces of all the
   executions of the same wrapper are logged to the same file.
-  ITPCore.LOGGING\_MODE=2
  
   Wrapper execution trace logging enabled. The execution trace of each
   execution of each wrapper is logged to a different file.

There is an additional configuration parameter ITPCore.LOGGING\_FILTER
which allows the user to filter out some specific types of trace events
and not log them. This parameter goes in the same configuration file as
ITPCore.LOGGING\_MODE. Possible values are the following:

-  ITPCore.LOGGING\_FILTER=0

   All execution trace events are logged. This is the default value.

-  ITPCore.LOGGING\_FILTER=1

   Only events which have associated a Trace action are logged. The log
   level will be ERROR when the handler is “raise error” or “retry and
   fail if error persists”, and WARN when the handler is “ignore error”.
     
-  ITPCore.LOGGING\_FILTER=2

   Only events not associated to a Trace action are logged.

.. note:: When executing wrappers developed with previous versions of
   ITPilot, the two last values has a different meaning:

   -  ITPCore.LOGGING\_FILTER=1
  
      Only events which have associated a custom error (a user defined
      error) are logged. The log level will be ERROR when the handler is
      “raise error” or “retry and fail if error persists”, and WARN when
      the handler is “ignore error” or “retry and ignore the error if it
      persists”.
   -  ITPCore.LOGGING\_FILTER=2
  
      Only events not associated to a custom error (a user defined error)
      are logged.
