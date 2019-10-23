===============================================
Required Libraries to Develop Stored Procedures
===============================================

To develop a stored procedure, add the following jar files to the
CLASSPATH of your environment:

-  :file:`{<DENODO_HOME>}/lib/vdp-server-core/denodo-vdp-server.jar`
-  :file:`{<DENODO_HOME>}/lib/vdp-client-core/denodo-vdp-parser.jar`

You may want to add the libraries Apache Commons Lang library (:file:`{<DENODO_HOME>}/lib/contrib/commons-lang.jar`) and Apache Commons IO (:file:`{<DENODO_HOME>}/lib/contrib/commons-io.jar`) to the classpath of your project. They contain many helper utilities that can ease the development of the procedures. When deploying a procedure, you do *not* need to import them nor include their content within its because they are already present in the classpath of the Virtual DataPort server. 


If the stored procedure relies on
third-party libraries, you have to do one of the following steps:

-  :doc:`Import the third-party jar files<../../../administration/importing_extensions/importing_extensions>` into Virtual DataPort and when creating the new stored procedure from the administration tool, select the jar of the procedure *and* the
   external jars.
-  Or copy the contents of the required jars into the jar that contains
   the stored procedure. You have to copy the contents of the required
   jars, not the jars themselves.
