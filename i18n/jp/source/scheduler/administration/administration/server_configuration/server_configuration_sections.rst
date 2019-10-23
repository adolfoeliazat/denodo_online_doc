========================================
Server Configuration Sections
========================================

This configuration area is divided in several sections.

|

Connection Details
=================================================================================

This section shows the name and port of the server that is using the
administration tool, and the login name of the authenticated user. The
links to configure the following features will be shown in the screen:

 

-  Import server configuration, projects, plugins and JDBC adapters from a
   file containing the metadata of a server (*Import* option).
   
   - Zip file to import: enter the path to the Scheduler backup file. That is, the file that
     contains the elements to recreate the metadata. If you have exported the metadata with the
     *Export with properties* option enabled, the zip file generated already contains the properties
     file inside, so you do not need to provide a single properties file (see next option), 
     unless you want to override the properties inside the zip file with the ones in the file 
     you provide.
   - Properties file to import (optional): if you have exported the metadata with the 
     *Export with properties* option enabled and the zip file to import does not include the 
     properties (or you want to override them), enter the path to the file with the ``.properties``.
     Properties in the file selected in this option have priority over the ones contained inside the zip file.
   
   The “Data import” dialog will then show 
   a tree with the projects and its elements included in the file.
   Use the check boxes to select the individual contents to import. It is
   possible to configure which kinds of elements (jobs, data sources,
   filter sequences, plugins and drivers) will be replaced by the ones
   included in the imported file if they already exist. This is useful for
   migration and backup purposes. Denodo Scheduler also provides scripts
   for importing the metadata (see appendix :doc:`/scheduler/administration/appendix/use_of_the_importexport_scripts_for_backup`).

-  Export projects (with their data sources, filter sequences and jobs), plugins, JDBC
   adapters and the server configuration (*Export* option). This is useful
   for migration and backup purposes. It generates a zip file including all
   the required information to restore the current server metadata. It is
   possible to choose what elements to export:


   -  All the projects (with all their elements), choosing the set of resources to export among the
      server configuration, the plugins and the JDBC adapters used by the
      elements of the exported projects. Additionally, by checking the
      option “Export plugins or drivers not used (only when all projects
      are selected)”, all the plugins and drivers will be included in the
      generated backup file, even though they are not used from any project
      element.
   -  A single project or a set of projects, also choosing the set of
      elements (data sources, filter sequences and jobs) of each project
      and the resources to export among the server configuration, the plugins
      and the JDBC adapters used by the chosen elements of the exported projects.
   -  You can include the elements on which the selected items depend by checking
      the option *Export dependencies*. For instance, if you select to export a job "Job1"
      that is configured with a data source "DS1" and you do not select this data source to
      be exported, by enabling this option it automatically will be included in the export file.
   -  You can include the permissions assigned for each role by checking
      the option *Export roles and permissions*. Note that permissions assigned to not selected (to be exported)
      projects and jobs will not be included in the export file.
   
   You can export the environment dependent properties (such as the connection data for data sources, etc.)
   separately in a different properties file by checking the option *Export with properties*. In this case,
   a zip file is generated, containing these two files:
   
   -  The backup (a ``.zip`` file). The values of the parameters that depend on the environment 
      are variables instead of the actual values.
   -  A file (with the extension ``.properties``) that contains the values of the variables.
   
   This option makes exporting and importing metadata across different environments, easier.

   The platform also provides scripts to run automatic backup copies (see
   appendix :ref:`Use of the Import/Export Scripts for Backup`). It is
   important to note that sensitive data (like passwords) are encrypted in
   the backup file.

.. _Change The Local User Password:

-  Change the local user password. The password for the local administrator
   user can be changed by clicking on the link *Change password*. The form
   for updating the password asks the user to enter the old password (Old
   password) and the new one duplicated (New password and Retype new
   password). The changes will be effective after pressing the “Accept”
   button, and the user may cancel the operation by pressing the button
   “Cancel”. Only the local administrator user can modify this password.

-  There are two ways of releasing space from the metadata database 
   of Scheduler:

   a. **Delete all reports**. Releases space by recreating the entire reports
      table in the database. All the reports will be deleted.
   b. **Compact reports**. Releases space by compressing the reports table in
      the database. This option is longer than the previous one, but it
      does not need to delete the reports. The more fragmented the table
      is, the more space it will release.
      
      .. note:: This option is only available when using Derby as database.


Server Connectivity Details
=================================================================================

 

The Scheduler server uses three port numbers for client communications:
the server execution port, the server stoppage port, and the auxiliary
port. These ports can be configured by choosing the link *Edit server
connectivity details*.

.. important::  When the Scheduler server and the Administration
   Tool or other clients are installed in different machines, you may need
   to change the interface where the server listens to connections. To
   change this, open the Denodo Control Center, open the
   "Configuration" dialog and change the "RMI Host" of the
   "JVM Options". See more details in the section :ref:`Denodo Platform Configuration` 
   of the Installation Guide. The reason for this is that in some
   environments, when listening to ‘localhost’, the server does not allow
   connections from remote hosts.
 

.. note:: Where the connection between customers and the Scheduler
   server has to go through a firewall, this must be configured to allow
   for access to the execution port and the auxiliary port.

The port changes will take effect the next time the Scheduler server
restarts.

|

Threads Pool
============

The Scheduler server allows you to execute various extraction jobs
simultaneously. Additionally, the VDP, ITP, and JDBC jobs allow the same
job to run different queries concurrently on the same data source,
varying the parameters.

 

The link *Edit threads pool* lets you change the concurrence
configuration of the Scheduler server. You can specify the maximum
number of jobs that the server will execute concurrently with the
parameter **Maximum number of concurrent jobs** (by default 20). A
change to the number of concurrent jobs will take effect the next time
the Scheduler server is restarted.

 

With regard to VDP-, ITP-, and JDBC-type jobs, the Scheduler server uses
a pool of reusable threads for managing the execution of multiple
queries which the same job can generate. The parameters that can be
configured are as follows:

-  **Default number of threads**. This represents the number of threads
   in the pool from which the inactive threads are reused (20 by
   default). Whilst there are fewer threads than these in the pool, new
   threads will continue to be created. When a thread is requested and
   the number of threads in the pool is the same as or more than this
   value, inactive threads are returned, if they exist; otherwise, new
   threads will continue to be created until obtaining the value
   established by the following parameter. Intuitively, this parameter
   indicates the number of threads that the system should have active
   simultaneously in normal load conditions.
-  **Maximum number of threads**. Represents the maximum number of pool
   threads (by default 60).
-  **Keep alive time** (ms). Specifies the maximum time in milliseconds
   that an inactive thread stays in the pool, if the number of threads
   exceeds the total indicated in the **Default number of threads** (by
   default 0). If the value is 0, the threads created above this value
   end, once the execution of their job has been completed. Otherwise,
   the ones that exceed the time specified by this parameter will end.


Mail Configuration
=================================================================================

 

The link *Edit mail configuration* allows you to modify the properties of the
outgoing mail server (:ref:`Handler Section`) used to send reports on the execution of
jobs. 

You can specify the following information:

-  **SMTP server**. Machine name in which the e-mail server runs.
-  **Port**. Port number in which the server runs. By default, 25.
-  **Protocol**. Protocol used to send the e-mails: SMTP or SMTPS.
-  **From**. The e-mail address used by Scheduler to send the mail.
-  **Subject**. The subject of the mail. The
   subject may contain variables, which will be replaced by their
   corresponding values at execution time (they take their value from the
   job data). The allowed variables are: ``projectName``, ``projectID``,
   ``jobName``, ``jobID``, ``jobStartTime`` and ``jobResult``, with the
   format ``@{<var_name>}``. It is possible to format ``jobStartTime``
   variable by specifying output date pattern. The syntax is
   ``@{jobStartTime,outputDateFormat:yyyy-MM-dd}``. As an example:

 
   .. code-block:: none
 
      Denodo Scheduler Notification - @{jobResult} [@{projectName}
      - @{jobName}_@{jobID}] at @{jobStartTime,outputDateFormat:yyyy-MM-dd HH:mm:ss}

-  **Trust servers**. If set to "*", all hosts are trusted. If set to a whitespace separated list of hosts, those hosts are trusted. Otherwise, it depends on the certificate the server presents.
-  **Enable TLS**. If true, enables the use of the STARTTLS command (if supported by the server) to switch the connection to a TLS-protected connection before issuing any login commands. Note that an appropriate trust store must be configured so that the client will trust the server's certificate. Defaults to false.
-  **Enable debug**. If true, it allows to trace the mail connection. In addition, it is necessary to add the following logger to the DENODO_HOME/conf/scheduler/log4j2.xml file:

   .. code-block:: xml
   
      <Logger name="com.denodo.scheduler.core.commons.utils.MailUtil" level="DEBUG" />


Additionally, if the outgoing mail server requires authentication, user
name (**Username**) and its password (**Password**) must be specified.

 

Once configured the mail properties, the link *Test mail configuration*
allows you to test if it is properly configured. You have to specify a
list of email addresses (separated by a “,”) a test mail will be sent to
and click on *Send*. If the configuration is right, those recipients
will receive an email with a body informing that it is a test email.

|

Virtual DataPort Settings
==========================

Denodo Scheduler servers can delegate the authentication of the users to
a Denodo Virtual DataPort server (see section :doc:`Authentication <../authentication/authentication>`).
Moreover, when Scheduler connects to Virtual DataPort to authenticate a
user, it also retrieves its roles (which determine the user access
rights in Scheduler).

 

In this way, Scheduler needs to know information about the Virtual
DataPort server used for authentication and roles retrieval. The link
Edit Virtual DataPort Settings allows specifying the following
information:

-  **Host**. Machine name in which the Virtual DataPort server runs.
-  **Port**. Port number in which the Virtual DataPort server runs.
-  **Database** (optional). Name of the Virtual DataPort server
   database, which should be used for the authentication (useful when
   the database uses LDAP authentication).

 

Changes in any of these parameters require the user to logout and login
again to take effect.

 

.. note:: As commented previously in this document, the non-local users,
   together with its roles, must be created in the Virtual DataPort server
   by an administrator user, and VDP-based
   authentication is only possible if connection with the configured
   Virtual DataPort server is possible (otherwise, only local-based
   authentication can take place).

Database Settings
==========================

Denodo Scheduler comes with an out-of-the-box embedded database (Apache Derby) where stores all of its metadata (projects, jobs, etc.).
By clicking the link Edit Database settings you can specify another database to store the metadata. You have to provide the following information:

-  **Driver class name**. Name of the Java class of the JDBC adapter to be used.
-  **Connection URI**. Database access URI.
-  **Username** (optional). Username for accessing the external database.
-  **Password** (optional). User password for accessing the external database.
-  **Database Name**. Select the database name which corresponds with the configured parameters. You can choose among these ones:

   -  Derby (embedded or server)
   
   -  MySQL
   
   -  Oracle
   
   -  Oracle 11
   
   -  PostgreSQL
   
   -  SQL Server

-  **Export metadata** (optional). By checking this parameter, it will export all your projects and elements, so that you can later import
   them in the new configured database. This is highly recommended if you want to keep your current metadata in the new database.
   
.. warning:: Never start a *non-clustered* instance (see next section to know how to configure clustering) against the same set of database tables that any other instance is running against. You may get serious data corruption, and will definitely experience erratic behavior.

Prior to configure these settings you need to include the new database drivers in DENODO_HOME/lib/scheduler-extensions and restart the Scheduler
server to load them. You also need to execute the scripts from DENODO_HOME/scripts/scheduler/sql to create the metadata tables used by Scheduler
in the new database. There are two script files for each supported database: <db_name>.sql and drivers_metadata.sql. 

For instance, if you want Oracle 12 to be the new database for the Scheduler metadata, you must choose ``Oracle`` in **Database Name** and then copy the .jar files from 
``DENODO_HOME/lib/extensions/jdbc-drivers/oracle-12c`` into ``DENODO_HOME/lib/scheduler-extensions``. You also need to connect to the desired Oracle 12 instance and execute
the scripts ``tables_oracle12.sql`` and ``drivers_metadata.sql`` located in ``DENODO_HOME/scripts/scheduler/sql/oracle-12c``. Similar steps should be done with any other of the supported databases.

Once restarted, you can configure these settings through this form and changes will be applied after restarting the server again.
If you checked the Export metadata check box, now you should import the generated backup to have all your previous metadata in the new configured database.

As a summary of the needed steps:

1. Copy the database jar files to ``DENODO_HOME/lib/scheduler-extensions``.

2. Execute the scripts from ``DENODO_HOME/scripts/scheduler/sql`` to create the metadata tables used by Scheduler in the new database.
   
   a. There are two script files for each supported database:
   
      i.  First, load ``tables_<db_name>.sql``
   
      ii. Then, load ``drivers_metadata.sql`` 

3. Restart the Scheduler server.

4. Use the Database settings form to configure the settings for the new database.

   a. Optionally, export the current metadata.
   
   b. Note that if there is any error (the jar files are not copied to the right folder, any setting is bad configured or the metadata tables are not already created in the new database), you will receive an alert message informing you about this situation and warning you that if you accept the changes your Scheduler server could not be started any more. Nevertheless, if you end up with an unusable Scheduler server, you can restore the default database settings by manually editing the ``DENODO_HOME/conf/scheduler/ConfigurationParameters.properties`` file and having the following properties like this:
   
.. code-block:: properties

   ComplexDataSource/driverClassName=org.apache.derby.jdbc.EmbeddedDriver
   ComplexDataSource/url=jdbc:derby:schedulerDB
   ComplexDataSource/user=derby
   ComplexDataSource/password=derby
   
   JobReportRepositoryFactory/className=com.denodo.scheduler.core.job.report.repository.DerbyJobReportRepositoryImpl

   org.quartz.threadPool.class=org.quartz.simpl.SimpleThreadPool

   org.quartz.jobStore.class=org.quartz.impl.jdbcjobstore.JobStoreTX   
   org.quartz.jobStore.driverDelegateClass=org.quartz.impl.jdbcjobstore.PostgreSQLDelegate
   org.quartz.jobStore.useProperties=false
   org.quartz.jobStore.dataSource=quartzds
   org.quartz.jobStore.tablePrefix=QRTZ_

   org.quartz.dataSource.quartzds.driver=org.apache.derby.jdbc.EmbeddedDriver
   org.quartz.dataSource.quartzds.URL=jdbc:derby:schedulerDB
   org.quartz.dataSource.quartzds.user=derby
   org.quartz.dataSource.quartzds.password=derby
   org.quartz.dataSource.quartzds.maxConnections=6
   org.quartz.dataSource.quartzds.validationQuery=values(1)

5. Restart the Scheduler server again.

   a. From now on, Scheduler is running against the new configured database.

6. If you exported the metadata in step 4, import it in the new database (as explained in :ref:`Connection Details`).


Cluster Settings
==========================

Clustering works by having each node of the cluster share the same database. 
So, when enabling this feature you also have to configure the Scheduler server to use the common database (see :ref:`Database settings`). As can be seen in the warning of the previous section, **you should never configure a non-clustered server to use the same database as any other server, so you must not configure a common database without enabling the clustering features** (note that the changes will be effective after restarting the server).

You can configure the following parameters:

-  **Cluster.** Enable it in order to turn on clustering features. As stated in the previous subsection, this property must be set to “true” if you are having multiple instances of Scheduler using the same set of database tables.
-  **Node identifier.** Can be any string, but must be unique for each Scheduler server in the cluster. You may use the value “AUTO” as the node identifier if you wish the identifier to be generated for you.
-  **Check interval.** Sets the frequency (in milliseconds) at which this instance checks-in with the other instances of the cluster. Affects the quickness of detecting failed instances. Recommended values are, for instance, 2000 or 3000 milliseconds.

These changes will take effect the next time the Scheduler server restarts.

.. warning:: If you run clustering on separate machines (i.e. if you are not running all the Scheduler server instances in the same machine), the clocks of all the machines where the instances of the Scheduler server are running must be synchronized using some form of time-sync service (daemon) that runs very regularly. The time-sync service used must assure that the clocks are within a second of each other. Otherwise, various strange behaviors can result, including nodes presuming other nodes have crashed and performing fail-over (duplicate firing) of jobs. To synchronize the clocks of all nodes in the cluster use a time service such as `NTP <http://www.ntp.org/>`_ (see also the `NIST Internet time service <https://www.nist.gov/pml/time-and-frequency-division/services/internet-time-service-its>`_).

Configuring a cluster of Scheduler servers is the key for a High Availability (HA) scenario and provides the following features:

-  Load-balancing: occurs automatically, with each node of the cluster firing jobs as quickly as it can. When a trigger’s firing time occurs, the first node to acquire it (by placing a lock on it) is the node that will fire it. Only one node will fire the job for each firing. It will not necessarily be the same node each time.
-  Fail-over: occurs when one of the nodes fails while in the midst of executing one or more jobs. When a node fails, the other nodes detect the condition and identify the jobs in the database that were in progress within the failed node and they are re-executed by the remaining nodes. Notice that the complete job would be re-executed, so if it has some exporters that generate files, they might be duplicated (unless they are configured to be overwritten). Besides, the recommendation is to export these files to a shared drive to be accessible to any server in the cluster.

Server configuration settings (the ones explained in this section) can be synchronized among servers by using the Import/Export feature (see :ref:`Connection details` and :doc:`Use of the Import/Export Scripts for Backup <../../appendix/use_of_the_importexport_scripts_for_backup>`) and including the configuration.

---------------------------------------------
How to Remove a Scheduler Server from Cluster
---------------------------------------------

If you have configured a cluster of several Scheduler servers and you want to remove one of them from the cluster, you will have to perform the following these steps:

-  Stop all the other Scheduler servers.
-  On the Scheduler server you want to remove from the cluster, go to :ref:`Cluster settings` and disable clustering.
-  As the current database is configured like the other servers that are in the cluster, you will have to modify its configuration in order to not get corrupted metadata (you should never configure a non-clustered server to use the same database as any other server).
   
   -  If this server is the last one in the cluster, you can keep the database configuration (as it is supposed to be the only server with that database configuration).

-  Start all Scheduler servers.

---------------------------
Scheduler HA Best Practices
---------------------------

If you have configured a cluster of several Scheduler servers, all of them must have access to the resources needed to execute the jobs (as any of the servers could execute any of the jobs).

- Configure the jobs to export their generated files (in a CSV, SQL and/or custom exporters) to a shared drive, so that those files are accessible to any server in the cluster.
- Modify the paths for uploaded files to point to a shared folder, so that the uploaded items can be accessible from any server in the cluster. 
  **In each server**, edit the :file:`{<DENODO_HOME>}/conf/scheduler/ConfigurationParameters.properties` to modify the following properties:
  
  - ``JDBCDataSource/userJdbcDriversDir``: stores the :ref:`JDBC drivers uploaded by the users <sched_drivers>`.
  - ``MetadataManagerImpl/pluginsFolder``: stores the :ref:`custom plugins uploaded by the users <sched_plugins>`.
  - ``CSVDataSourceStorer/csvFolder``: stores the CSV files uploaded by the users when creating CSV data sources.


.. _sched_admin_guide_server_configuration_kerberos_settings:

Kerberos Settings
==========================

These settings affect both the :doc:`Single Sign-On authentication <../web_configuration/kerberos_configuration/kerberos_configuration>` from the Scheduler administration tool 
and the Virtual DataPort data sources to use Kerberos authentication (see :ref:`VDP Data Sources` for more information).

.. important:: In order to use Single Sign-On in Scheduler, you must first :doc:`configure </scheduler/administration/administration/web_configuration/kerberos_configuration/kerberos_configuration>` in the Scheduler administration tool.

1. Select **Use Kerberos**.

#. In the box **Server Principal** enter the “Service Principal Name” (SPN)
   used to create the *keytab* file. That is, the SPN with the Fully
   Qualified Domain Name (FQDN) of the server where the Active Directory is
   running. For example, "*HTTP/denodo-prod.subnet1.contoso.com\@CONTOSO.COM*".
   
   .. note:: It is not necessary for the SPN of the Scheduler server to start with ``HTTP``. This is only mandatory when web browsers request a Kerberos ticket, as they do it for the service "*HTTP/<host name of the URL you are accessing>*". Here, the Kerberos ticket is requested by the Scheduler administration tool, not by the browser. However, in the case of the Scheduler administration tool, it is mandatory that the SPN starts with ``HTTP``, since in this case the browser requests the Kerberos ticket. Therefore, on the Scheduler server you could use a SPN such as "*SCHED/denodo-prod.subnet1.contoso.com\@CONTOSO.COM*", if you wish.

#. In the box **Keytab file** enter the path to the *keytab* file.

#. Leave the **Kerberos configuration file** box empty unless the host
   where this Scheduler server runs does not belong to a
   Kerberos realm (e.g. a Windows Active Directory domain). If this host
   does *not* belong to a Kerberos realm, do one of the following:

   a. Enter the path to the ``krb5.conf`` or ``krb5.ini`` file with the
      Kerberos settings.
   b. Or follow the steps described in the appendix :ref:`Using Kerberos Authentication in Scheduler Without Joining a Kerberos Realm` 
      of the Installation Guide.
	  
   .. note:: This is the only setting you have to configure to use Kerberos authentication in the Virtual DataPort data sources

#. We recommend selecting the check box **Activate Kerberos debug mode**
   the first time you set up Kerberos in case you run into any issues. Once
   Kerberos has been set up, disable this.


   When this option is enabled, the Java Runtime Environment logs messages
   related to Kerberos in the standard output but not in the log files. To
   see these messages, launch the Scheduler server from the
   command line with the script
   :file:`{<DENODO_HOME>}/bin/scheduler_startup`. 
   
   .. important:: In Windows environments, you must redirect the server's output to a file. Otherwise, you will not be able to check the log messages related to Kerberos.
   
   To redirect these debug messages to a file, start the Server like this:
   
   For Windows:
   
   .. code-block:: batch
   
      cd <DENODO_HOME>
      cd bin
      scheduler_startup.bat > scheduler_kerberos_logging.log 2>&1
      
   For Linux:
   
   .. code-block:: bash
   
      cd <DENODO_HOME>
      cd bin
      ./scheduler_startup.sh > scheduler_kerberos_logging.log 2>&1

Changes in this parameter requires the server to be restarted to take effect.
