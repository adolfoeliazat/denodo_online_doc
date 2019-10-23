================================
Installing the SAP JCo Connector
================================

In order to retrieve data from SAP ERP (BAPI data sources) or from SAP
BW / SAP BI (with multidimensional data sources with the BAPI adapter),
install the SAP Java Connector 3.0 (SAP JCo).

SAP provides different JCo libraries for several platforms. The JCo
library you install must work with the CPU manufacturer and architecture
on which the Denodo server runs:

-  If Virtual DataPort runs on a 32-bit O.S, download the 32-bit
   connector.
-  If Virtual DataPort runs on a 32-bit JVM, on a 64-bit O.S, download
   the 32-bit connector.
-  If Virtual DataPort runs on a 64-bit JVM, download the 64-bit
   connector.

The following subsections explain:

-  :ref:`How to install SAP JCo on Windows<Installing SAP JCo on
   Windows>`.
-  :ref:`How to install SAP JCo on Linux<Installing SAP JCo on
   Linux>`.

Installing SAP JCo on Windows
==============================================================

Follow these steps to install the SAP JCo connector on Windows:


#. Obtain the appropriate driver for your architecture. It can be
   downloaded from https://support.sap.com/connectors, section SAP Java
   Connector > Tools & Services.


#. Uncompress the downloaded package in a temporary directory.

#. Create the directory ``sap-jco-connector`` inside
   ``<DENODO_HOME>\extensions\thirdparty\lib\``.

#. Copy the following files to ``<DENODO_HOME>\extensions\thirdparty\lib\sap-jco-connector\``

   -  ``sapjco3.jar``
   -  ``sapjco3.dll``

#. SAP JCo depends on the "Microsoft Visual C++ 2010 Redistributable
   Package" so you need to install it:

   i. Open the page to download the `Microsoft Visual C++ 2010 Redistributable
      Package <https://www.microsoft.com/en-us/download/details.aspx?id=26999>`_.

   #. Select one of the following platform-specific files depending on your
      scenario:

      a. Virtual DataPort server running on a 32-bit O.S: vcredist\_x86.exe
      #. Virtual DataPort server running on a 32-bit JVM, on a 64-bit O.S:
         vcredist\_x86.exe
      #. Virtual DataPort server running on a 64-bit JVM: vcredist\_x64.exe
      #. Itanium system: vcredist\_IA64.exe

   #. Execute the downloaded file and follow the installation instructions.

#. To test that the JCo connector is properly installed, open a command prompt and execute
   the following:

   .. code-block:: batch

      cd <DENODO_HOME>
      cd extensions\thirdparty\lib\sap-jco-connector
      ..\..\..\..\jre\bin\java.exe -jar sapjco3.jar -stdout

   You should see something like this:

   .. code-block:: none
      :emphasize-lines: 11,12

      Java Runtime:
       Operating System:         Windows 8.1 6.3 for amd64
       Java VM:                  1.7.0_80 Oracle Corporation
       Java Codepage:            Cp1252
      Versions:
       JCo API:                  3.0.4 (2009-12-10)
       JCo middleware name:      JavaRfc
       JCo middleware:           2.2.1
       JCo middleware native:    720.29
      Paths:
       JCo classes:              C:\Denodo\DenodoPlatform7.0\extensions\thirdparty\lib\sap-jco-connector\sapjco3.jar
       JCo library:              C:\Denodo\DenodoPlatform7.0\extensions\thirdparty\lib\sap-jco-connector\sapjco3.dll

   Note that in the last two lines, you should see the path to the files sapjco3.jar and sapjco3.dll. If you see something like "JCo library: not loaded, caused by java.lang.UnsatisfiedLinkError: ... Can't load IA 32-bit .dll on an AMD 64-bit platform" it means that the file sapjco3.dll is the 32-bit version of the file and you should copy the 64-bit one.

   Note that you are launching the Java Runtime Environment installed along
   with the Denodo Platform. If you are going to launch the Denodo Platform
   with another JRE, you have to execute this command with that JRE to make
   sure that at runtime, the SAP JCo connector will work.

Installing SAP JCo on Linux
==============================================================

Follow these steps to install the SAP JCo connector on Linux:

#. Obtain the appropriate driver for your architecture. It can be
   downloaded from http://service.sap.com/connectors, section SAP Java
   Connector > Tools & Services.

#. Uncompress the downloaded package in a temporary directory.

#. Create the directory ``sap-jco-connector`` inside
   :file:`{<DENODO_HOME>}/extensions/thirdparty/lib/`

#. Copy the following files to the directory
   :file:`{<DENODO_HOME>}/extensions/thirdparty/lib/sap-jco-connector/`

   -  ``sapjco3.jar``
   -  ``libsapjco3.so``

#. To test that the JCo connector is working, execute the following
   command:

   .. code-block:: batch

      cd <DENODO_HOME>
      cd extensions/thirdparty/lib/sap-jco-connector
      ../../../../jre/bin/java -jar sapjco3.jar -stdout

   You should see something like this:

   .. code-block:: none
      :emphasize-lines: 11,12

      Java Runtime:
       Operating System:         Linux 2.6.32-358.el6.x86_64 for amd64
       Java VM:                  1.8.0_152 Oracle Corporation
       Java Codepage:            UTF8
      Versions:
       JCo API:                  3.0.4 (2009-12-10)
       JCo middleware name:      JavaRfc
       JCo middleware:           2.2.1
       JCo middleware native:    720.29
      Paths:
       JCo classes:              /opt/denodo/denodo-platform-7.0/extensions/thirdparty/lib/sap-jco-connector/sapjco3.jar
       JCo library:              /opt/denodo/denodo-platform-7.0/extensions/thirdparty/lib/sap-jco-connector/sapjco3.so

   Note that in the last two lines, you should see the path to the files sapjco3.jar and sapjco3.dll. If you see something like "JCo library: not loaded, caused by java.lang.UnsatisfiedLinkError: ... Can't load IA 32-bit .dll on an AMD 64-bit platform" it means that the file sapjco3.dll is the 32-bit version of the file and you should copy the 64-bit one.

   Note that you are launching the Java Runtime Environment installed
   with the Denodo Platform. If you are going to launch the Denodo
   Platform with another JRE, you have to execute this command with that
   JRE to make sure that at runtime, the SAP JCo connector will
   work.

   If the library is properly installed, this command runs without error
   and provides information about the installed JCo libraries.
