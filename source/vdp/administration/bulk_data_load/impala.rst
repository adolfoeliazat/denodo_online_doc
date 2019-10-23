======
Impala
======

To be able to use the Impala API to perform bulk data loads, first install the Hadoop client libraries on the host where the Virtual DataPort server runs. To do it, follow these steps:

#. Install the Java Development Kit version 8 (JDK) on the host of the Virtual DataPort server. The Hadoop client requires it. A Java Runtime Environment (JRE) is not valid.

#. Set the JAVA_HOME environment variable to point to the path of this JDK. On Windows, it is quite common for this path to contain spaces, but Hadoop does not support them. If the JDK is installed on a path with spaces, set the environment variable JAVA_HOME to C:\\Java and create a symbolic link:

   .. code-block:: bash
   
      mklink /d "C:\Java" "C:\Program Files\Java\jdk1.8.0_152"

   If you get a privileges error executing this command, click the Windows menu, search for **Command Prompt**, right-click on it and click **Run as administrator**.

#. Find out the specific version of Hadoop you are connecting to and go to the `Apache Hadoop site <https://hadoop.apache.org/releases.html>`_ and download the binary file corresponding to the version you use. For example, `hadoop-2.9.0.tar.gz <http://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-2.9.0/hadoop-2.9.0.tar.gz>`_.

#. Extract this package in the host where the Virtual DataPort server runs.

   On Linux, run this to uncompress it:

   .. code-block:: bash
   
      tar -xzf <archive_name>.tar.gz

#. On Windows, check that the file :file:`<HADOOP_HOME>/bin/winutils.exe` exists. Otherwise, the connection to Hadoop will fail.

#. Set the following environment variable in the system:

   .. code-block:: bash

      HADOOP_HOME=<folder where you uncompressed the Hadoop client package>

#. Depending on the authentication method you want to use to connect to Hadoop when uploading the files for bulk data load, choose one of these options:

   a. To use standard authentication -- not Kerberos -- to connect to Hadoop, set this environment variable in the host where the Virtual DataPort server runs:

      .. code-block:: bash
 
         HADOOP_USER_NAME=<username in Hadoop>

      This user name must have read and write privileges over the HDFS folder to which the data will be uploaded.

      .. note:: If in the same host, there are several applications that connect to Hadoop, instead of setting environment variables, create a script that sets the variables ``HADOOP_USER_NAME`` and ``HADOOP_HOME`` and then, depending on the platform, invokes :file:`hadoop.cmd` or :file:`hadoop`. Later, in the configuration of bulk data load, point to this script.

   b. To use Kerberos authentication to connect to Hadoop, do the following changes:
   
      i. Edit the file :file:`<HADOOP_HOME>/etc/hadoop/core-site.xml` and add the following properties:

         .. code-block:: xml

            <property>
                <name>hadoop.security.authentication</name>
                <value>kerberos</value>
            </property>
            <property>
                <name>hadoop.security.authorization</name>
                <value>true</value>
            </property>

      #. Create a script: 
      
         -  On Linux, create the file :file:`{<DENODO_HOME>}/renew_kerberos_ticket_for_bulk_data_load.sh` with this content:
         
            .. code-block:: bash
            
               #!/bin/bash
               kinit -k -t "<path to the keytab file>" <Kerberos principal name of the Hadoop service>
               $HADOOP_HOME/hadoop "$@" 

            After creating the file, execute this:
            
            .. code-block:: bash
               
               chmod +x <DENODO_HOME>/renew_kerberos_ticket_for_bulk_data_load.sh

         -  On Windows, create the file :file:`{<DENODO_HOME>}/renew_kerberos_ticket_for_bulk_data_load.bat` with this content:
         
            .. code-block:: batch
            
               @echo off
               kinit -k -t "<path to the keytab file>" <Kerberos principal name of the Hadoop service>
               %HADOOP_HOME%\hadoop.cmd %*

         Replace ``<path to the keytab file>`` with the path to the keytab file that contains the keys to connect to the Hadoop server.
            
         By invoking kinit before invoking the Hadoop client, we make sure the system has a valid Kerberos ticket to be able to connect to Hadoop.
         
#. On the administration tool, edit the JDBC data source and click the tab **Read & Write**. Select **Use bulk data load APIs** and:

   -  In the **Hadoop executable location** box, enter the path to the file :file:`hadoop.cmd` on Windows, and :file:`hadoop` on Linux. If in the previous step you created the script ``renew_kerberos_ticket_for_bulk_data_load``, put the path to this file.
   
   -  In **HDFS URI**, enter the URI to which Virtual DataPort will upload
      the data file. For example:
      ``hdfs://acme-node1.denodo.com/user/admin/data/``

|

At runtime, when a query involves a bulk load to Impala, Virtual DataPort does two things:

1. It uploads the temporary data files to the Impala distributed file system (HDFS). 

   To do this, it executes the command ``hadoop.cmd fs put...`` locally, on the host where the Virtual DataPort server runs.

   Depending on the configuration of Impala, sometimes you need to export the environment variable ``HADOOP_USER_NAME`` with the user name of the data source so the next step of the process can be completed.

   To execute this command, Virtual DataPort does *not* use the credentials of the data source. It just executes that command. Therefore, the HDFS system has to be configured to allow connections from this host. For example, by setting up an SSH key on the host where Virtual DataPort runs that allows this connection or by allowing connections through Kerberos.

#. It creates the tables in Impala associated to the data uploaded in the step #1.
    
   To do this, it connects to Impala using the credentials of the JDBC data source and executes ``LOAD DATA INPATH ...``. To complete this command, the user account of the JDBC data source needs to have access to the files uploaded in the first step.

