=========
Appenders
=========

.. toctree::
   :hidden:
   
   configuring_custom_appenders.rst
   custom_appenders_and_logger_additivity.rst

If no specific appenders are configured for the wrapper execution trace
log categories, a default FileAppender will be created, using as
template the FILEOUT appender found in the log4j configuration file.
Most of the configuration is inherited from FILEOUT, in particular the
folder path of the output file. The name of the output log file itself
is determined as follows:

-  If all the executions of the wrapper are logged to same file
   (ITPCore.LOGGING\_MODE=1), the name of the file will be the same as
   the name of the wrapper.
-  If each execution is logged to a different file
   (ITPCore.LOGGING\_MODE=2), the name of each log file will be the name
   of the wrapper + a timestamp (format ‘yyyyMMddHHmmssSSS’) + an
   execution id, e.g. “TestWrapper.20110323143206698.1”.

.. note:: The default appender is added to the wrapper log categories
   with the “additivity” property set to “false”, which means that the log
   messages are not sent to the appenders of the ancestor categories (i.e.
   the wrapper execution trace log messages are not logged in the Wrapper
   Server/Wrapper Generation Tool main log file).


