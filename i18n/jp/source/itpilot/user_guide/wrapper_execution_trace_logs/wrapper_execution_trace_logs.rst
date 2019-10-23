============================
Wrapper Execution Trace Logs
============================

.. toctree::
   :hidden:
   
   enabling_wrapper_trace_logging/enabling_wrapper_trace_logging.rst
   log_categories_and_effective_log_level/log_categories_and_effective_log_level.rst
   appenders/appenders.rst

It is possible to log the execution traces of wrappers, both of wrappers
which are deployed in the Wrapper Server and of wrappers executed from
the Wrapper Generation Tool. Logging can be enabled globally for all
wrappers, or just for specific ones. The user can choose between logging
all the executions of the same wrapper into the same log file, and
logging each execution separately to different files, where each file
name is distinguished by the start date and an identifier of the
execution.
