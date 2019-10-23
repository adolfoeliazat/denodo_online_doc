============================================
Required Libraries to Develop Custom Filters
============================================

To develop a custom filter, add the following jar files to the CLASSPATH
of your environment:

-  :file:`{<DENODO_HOME>}/lib/contrib/denodo-commons-connection-util.jar`
-  :file:`{<DENODO_HOME>}/lib/contrib/denodo-commons-util.jar`

If the custom filter relies on
third-party libraries, you have to do one of the following steps:

-  :doc:`Import the third-party jar files<../../../administration/importing_extensions/importing_extensions>` into Virtual DataPort and when importing the new custom filter from the administration tool, select the jar with the custom filter *and* the
   external jars.
-  Or copy the contents of the required jars into the jar that contains
   the stored procedure. You have to copy the contents of the required
   jars, not the jars themselves.
