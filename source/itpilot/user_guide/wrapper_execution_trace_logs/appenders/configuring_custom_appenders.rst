================================
Configuring Custom Appenders
================================

If the user wants more control over the logging of wrapper execution
trace events, he can define his own custom appender for the trace log
categories. He may use a regular log4j
org.apache.log4j.RollingFileAppender, but this suffers from a
limitation: execution trace messages produced by wrappers will all be
sent to the same file, even as the value of the ITPCore.LOGGING\_MODE
parameter is 1 o 2.



Another possibility consists in using a
com.denodo.itp.component.core.event.RollingFileWrapperAppender. This
class inherits the properties of a FileAppender but adds some extra
functionality.



RollingFileWrapperAppender lets the user specify a directory path or a
pattern as the value of the File parameter.

-  If a directory path is given, it will be used to store the log files,
   and the name of the log files themselves will be generated as if
   using the default appender (see section :ref:`Appenders`).
-  If a pattern is given, then “%w” in the pattern will be substituted
   by the file name (without extension) which the default appender would
   use. For example, if File value is “/tmp/Wrapper\_%w.rst”,
   ITPCore.LOGGING\_MODE=1 and the wrapper name is “Test”, the log file
   for this wrapper will be “/tmp/Wrapper\_Test.rst”. Note that if the
   pattern does not contain “%w”, then the file name will be constant
   and the execution traces produced by wrappers will all be sent to the
   same file.



RollingFileWrapperAppender also gives the user precise control over the
formatting of wrapper execution trace events. For this purpose, instead
of the regular log4j “org.apache.log4j.PatternLayout”, a
“com.denodo.itp.component.core.event.ScriptEventPatternLayout” should be
used. This latter class inherits from PatternLayout and adds the new
optional parameter MessagePattern, which allows to pre-format wrapper
trace events before applying the ConversionPattern layout. The following
syntax is used for MessagePattern:

-  **%c** represents the component name.
-  **%m** represents the message of the trace event.
-  **%t** represents the exception associated to event (if any).
-  **%e** represents the error code associated to the event (if any).

If no MessagePattern is specified, or if a PatternLayout is used instead
of a ScriptEventPatternLayout, the default format will be “%c - %m”. In
any case, the result of applying the MessagePattern will be substituted
for occurrences of “%m” in the ConversionPattern. (Note that
occurrences of “%m” in the ConversionPattern do not mean the original
trace message, as they do in the MessagePattern).


.. code-block:: xml
   :caption: RollingFileWrapperAppender with ScriptEventPatternLayout
   :name: RollingFileWrapperAppender with ScriptEventPatternLayout

   <appender name="WRAPPERSFILEOUT" class="com.denodo.itp.component.core.event.RollingFileWrapperAppender">
       <param name="File" value="c:/Temp/%w.log" />
       <param name= "Append" value= "true " />
       <param name= "MaxFileSize" value= "10MB " />
       <param name= "MaxBackupIndex" value= "7" />
       <param name= "encoding" value= "UTF-8" />
       <layout  
      class="com.denodo.itp.component.core.event.ScriptEventPatternLayout">
           <param name="ConversionPattern"
       value="%-4r [%t] %-5p %d{yyyyMMddHHmmssSSS} %c %x - %m  %n"/>
           <param name="MessagePattern" value="[%m] --[%c]"/>
       </layout>
   </appender>
   
   ...
   
   <logger name="com.denodo.itpilot.logging.wrappers">
       <level value="info"/>
       <appender-ref ref="WRAPPERSFILEOUT" /> 
   </logger>

`RollingFileWrapperAppender with ScriptEventPatternLayout`_ shows an
example of a custom RollingFileWrapperAppender which uses a
ScriptEventPatternLayout with a non-default MessagePattern. After its
definition, the appender is tied to a logger in the wrapper trace log
category.
