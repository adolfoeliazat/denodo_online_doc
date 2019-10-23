======================================
Custom Appenders and Logger Additivity
======================================

By default, each logging request for a given logger will be forwarded to
all the appenders of that logger as well as the appenders of its
ancestors, i.e. appenders are inherited “additively” in the logger
hierarchy.



Therefore, if a custom appender is added to a logger in the
“com.denodo.itpilot.logging.wrappers” or
“com.denodo.itpilot.logging.wrappers.<wrapper\_name>” categories, then
the log messages sent to this appender will also be sent to the ancestor
logger appenders (i.e. the execution trace log messages will also be
logged in the Wrapper Server/Wrapper Generation Tool main log files).



It is possible to override the default behavior so that appender
accumulation is no longer additive by setting the additivity parameter
of a logger to false. If a logger has the additivity parameter set to
false, then logging requests will not be sent to the appenders of any of
the ancestors of that logger. In particular, this can be done with
loggers in the “com.denodo.itpilot.logging.wrappers” or
“com.denodo.itpilot.logging.wrappers.<wrapper\_name>” categories.



.. note:: There is an exception to this. Due to the internal
   implementation of RollingFileWrapperAppender, if appenders of this type
   are configured for loggers of the “com.denodo.itpilot.logging.wrappers”
   and “com.denodo.itpilot.logging.wrappers.<wrapper\_name>” categories,
   then the messages sent to the appender of the
   “com.denodo.itpilot.logging.wrappers.<wrapper\_name>” logger are never
   sent to the appender of the “com.denodo.itpilot.logging.wrappers”
   logger, independently of the value of the parameter additivity.



A series of examples will clarify this. In the examples, the referenced
WRAPPERSFILEOUT and SPECIFICWRAPPERFILEOUT appenders are
RollingFileWrapperAppenders. For the situation depicted in
`Additivity="false" for the logger of a specific wrapper`_, the
execution trace of the wrapper "testcustomerrors" will be logged in
SPECIFICWRAPPERFILEOUT, and the execution trace of the rest of the
wrappers in WRAPPERSFILEOUT and FILEOUT. For the one in
`Additivity="false" for the logger of the general wrapper category`_,
the execution trace of the wrapper “testcustomerrors” will be logged in
SPECIFICWRAPPERFILEOUT, and the execution trace of the rest of the
wrappers in WRAPPERSFILEOUT. And for the one in `Loggers with default
additivity`_., the execution trace of the wrapper “testcustomerrors”
will be logged in SPECIFICWRAPPERFILEOUT and FILEOUT, and the execution
trace of the rest of the wrappers in WRAPPERSFILEOUT and FILEOUT.

.. code-block:: xml
   :caption: Additivity="false" for the logger of a specific wrapper
   :name: Additivity="false" for the logger of a specific wrapper

   <logger name="com.denodo.itpilot.logging.wrappers">
       <level value="debug"/>
       <appender-ref ref="WRAPPERSFILEOUT" /> 
   </logger>
   
   <logger name="com.denodo.itpilot.logging.wrappers.testcustomerrors" 
           additivity="false">
       <level value="debug"/>
       <appender-ref ref="SPECIFICWRAPPERFILEOUT" />
   </logger>    
   
   <root>
       <level value ="debug" />
       <appender-ref ref="FILEOUT" />
   </root>
   
   

.. code-block:: xml
   :caption: Additivity="false" for the logger of the general wrapper category
   :name: Additivity="false" for the logger of the general wrapper category

   <logger name="com.denodo.itpilot.logging.wrappers" additivity="false">
       <level value="debug"/>
       <appender-ref ref="WRAPPERSFILEOUT" /> 
   </logger>
   
   <logger name="com.denodo.itpilot.logging.wrappers.testcustomerrors">
       <level value="debug"/>
       <appender-ref ref="SPECIFICWRAPPERFILEOUT" />
   </logger>    
   
   <root>
       <level value ="debug" />
       <appender-ref ref="FILEOUT" />
   </root>

.. code-block:: xml
   :caption: Loggers with default additivity
   :name: Loggers with default additivity

   <logger name="com.denodo.itpilot.logging.wrappers">
       <level value="debug"/>
       <appender-ref ref="WRAPPERSFILEOUT" /> 
   </logger>
   
   <logger name="com.denodo.itpilot.logging.wrappers.testcustomerrors">
       <level value="debug"/>
       <appender-ref ref="SPECIFICWRAPPERFILEOUT" />
   </logger>    
   
   <root>
       <level value ="debug" />
       <appender-ref ref="FILEOUT" />
   </root>
