======================================
Log Categories and Effective Log Level
======================================

New log categories have been added to the log4j subsystem (`Apache Log4j <http://logging.apache.org/log4j/>`_). They serve to configure which
wrappers fall under the new logging regime. They are specified in the
log4j configuration file, which is <DENODO\_HOME>/conf/vdp/log4j.xml for
the Wrapper Server and <DENODO\_HOME>/conf/itp-admin-tool/log4j.xml for
the Wrapper Generation Tool.



-  The log4j category for all the wrappers is
   "com.denodo.itpilot.logging.wrappers". For example, to set the
   execution trace log level to WARN for all the wrappers:
 
 .. code-block:: xml
 
    <logger name="com.denodo.itpilot.logging.wrappers">
       <level value="WARN"/>
    </logger>

-  The log4j category for a single wrapper is
   "com.denodo.itpilot.logging.wrappers.<wrapper\_name>". For example,
   to set the execution trace log level to DEBUG for the wrapper
   "testwrapper":
 
 .. code-block:: xml
 
    <logger name="com.denodo.itpilot.logging.wrappers.testwrapper">
       <level value="DEBUG"/>
    </logger>

.. note:: Wrapper names must always be specified in lowercase when
   configuring the log4j file, even if the wrapper was named in uppercase
   when it was created.

For the Wrapper Server, the log level configured in log4j for the
aforementioned categories will be used. Note that if not log level is
set for these categories, then the log level set for ancestor categories
will be inherited.


When executing a wrapper from the Wrapper Generation Tool, there is an
additional thing to consider: if the log level selected in Wrapper
Tester window is more restrictive than the one specified in the log4j
file, the former will be used instead of the latter. For example:

-  If the log level configured in the log4j configuration file is DEBUG,
   but the wrapper is executed from the Wrapper Generation Tool with log
   level ERROR, then the trace will be logged using ERROR as log level.
-  If the log level configured in the log4j configuration file is INFO,
   and the wrapper is executed from the Wrapper Generation Tool with log
   level TRACE, then the trace will be logged using INFO as log level.

Given that TRACE is the less restrictive log level, selecting it in the
Wrapper Tester window means that the level configured in the log4j file
will be applied without changes.
