=============
Export Script
=============

The export script exports the metadata of a Virtual DataPort Server. The
features of this script are equivalent to the “Export” and “Export
database” dialogs of the Administration Tool (see section :ref:`Exporting and
Importing the Server Metadata`).

The metadata can be exported to:

-  A file that contains all the VQL statements to recreate the exported
   elements (option ``--file``)

-  Two files (option ``--property includeProperties``):

   a. A file that contains the VQL statements of all the elements of the
      database. The values of the parameters that depend on the environment
      are variables instead of the actual values.
   b. And, a properties file that contains the values of the variables.
      See more about this in the section :ref:`Export to a File with
      Properties`.




-  Or, a **Repository**: the metadata is stored in a set of files, where
   each file contains the VQL statement to create an element (option
   ``--repository``).

   After the exporting process, there will be a folder for each virtual
   database of the Server. In each of these folders, there will be a
   folder for each type of element: folders, data sources, views, etc.
   And in them, a file for each element, which will contain the VQL
   statement to create that element. This is useful if you want to use a
   revision control system such as Version Control System (VCS) to keep
   track of the changes in the metadata and the configuration of a
   Server.

   .. note :: If you want to store the metadata of the Server in a VCS, we
     recommend using the VCS integration of Virtual DataPort (see section
     :ref:`Version Control Systems Integration`). However, you may need to
     export to repository if your VCS is not supported.


This script is located in the directory :file:`{<DENODO_HOME>}/bin` and its
syntax is the following:

.. code-block:: bnf

   export --login <login> [ --password <password> ]
          [ --configurationfile <filename> ]
          --server [//]<host>:[<port>][/<database name>] [ --nobom ]
          [ --itponly ]
          {   --file <filename>
            | --repository <path to repository>
              [ {   --element <path to element>
                 | --repository-element <identifier path> }
              ]*
          }
          [ --singleuser ] [ -P <property name>=<property value> ]*

.. include:: <isotech.txt>

.. We include this to be able to put |minus|\ |minus|

.. table:: Parameters of the export script
   :name: Parameters of the export script

   +------------------------------+----------------------------------------------------------+
   | Parameter Name               | Description                                              |
   +==============================+==========================================================+
   | -l                           | Login name used to connect to the Server.                |
   |                              | ``<login>`` has to be                                    |
   | -\\-login <login>            | an *administrator* if                                    |
   |                              | you want to export all                                   |
   |                              | the metadata of the                                      |
   |                              | Server.                                                  |
   +------------------------------+----------------------------------------------------------+
   | -p                           | Password used to                                         |
   |                              | connect to the Server.                                   |
   | -\\-password <password>      |                                                          |
   |                              | Use the script ``encrypt_password`` to encrypt the       |
   |                              | password. That way you can avoid entering it in plain    |
   |                              | text.                                                    |
   |                              |                                                          |
   |                              | E.g. ``--password encrypted:Gr16MjvuXhRzPtPH/yTXHw==``   |
   |                              |                                                          |
   |                              | If you do not provide the parameter ``-p`` nor ``-cf``,  |
   |                              | the script will prompt you for your password.            |
   +------------------------------+----------------------------------------------------------+
   | -cf                          | File with one line that contains this:                   |
   |                              |                                                          |
   | -\\-configurationfile        | ``password=<password>``                                  |
   | <filename>                   |                                                          |
   |                              | or this:                                                 |
   |                              |                                                          |
   |                              | ``password=encrypted:<encrypted_password>``              |
   |                              |                                                          |
   |                              | Use the script ``encrypt_password`` to encrypt the       |
   |                              | password. That way you can avoid entering it in plain    |
   |                              | text.                                                    |
   |                              |                                                          |
   |                              | Incompatible with -p.                                    |
   +------------------------------+----------------------------------------------------------+
   | -h                           | URI of the Server                                        |
   |                              | (<host>:[<port>][/<database name>])                      |
   | -\\-server                   |                                                          |
   | <URI of the Server>          | If ``<database_name>``                                   |
   |                              | is present, the script                                   |
   |                              | exports the metadata of                                  |
   |                              | ``<database_name>``. If                                  |
   |                              | it is missing, the                                       |
   |                              | script exports the                                       |
   |                              | metadata of all the                                      |
   |                              | virtual databases and                                    |
   |                              | also the settings of                                     |
   |                              | the Server.                                              |
   +------------------------------+----------------------------------------------------------+
   | -f                           | Name of the target file                                  |
   |                              | where the VQL                                            |
   | -\\-file <filename>          | statements will be                                       |
   |                              | stored.                                                  |
   |                              |                                                          |
   |                              | This option is not                                       |
   |                              | compatible with the                                      |
   |                              | option ``--repository``                                  |
   |                              | (``-r``)                                                 |
   +------------------------------+----------------------------------------------------------+
   | -nb                          | If you add this                                          |
   |                              | parameter, the script                                    |
   | -\\-nobom                    | generates the output                                     |
   |                              | files *without* the                                      |
   |                              | “Byte order mark”                                        |
   |                              | (BOM).                                                   |
   |                              |                                                          |
   |                              | By default, the script                                   |
   |                              | adds the BOM mark to                                     |
   |                              | the output files to                                      |
   |                              | indicate that they are                                   |
   |                              | encoded in UTF-8.                                        |
   |                              | Thanks to this mark,                                     |
   |                              | the applications that                                    |
   |                              | have to read these                                       |
   |                              | files can quickly                                        |
   |                              | detect their encoding                                    |
   |                              | without having to guess                                  |
   |                              | it. However, there are                                   |
   |                              | applications that                                        |
   |                              | cannot handle the BOM.                                   |
   |                              | In that case, add this                                   |
   |                              | parameter.                                               |
   +------------------------------+----------------------------------------------------------+
   | -i                           | Export only WWW                                          |
   |                              | wrappers (ITPilot                                        |
   | -\\-itponly                  | wrappers).                                               |
   |                              |                                                          |
   |                              | **Note**: to use this                                    |
   |                              | option, you have to                                      |
   |                              | provide a                                                |
   |                              | ``<database_name>`` in                                   |
   |                              | the ``--server``                                         |
   |                              | parameter.                                               |
   +------------------------------+----------------------------------------------------------+
   | -r                           | Path to the directory                                    |
   |                              | where the Repository                                     |
   | -\\-repository               | will be created or                                       |
   | <path to repository>         | where it already                                         |
   |                              | exists.                                                  |
   |                              |                                                          |
   |                              | This option is not                                       |
   |                              | compatible with the                                      |
   |                              | option ``--file``                                        |
   |                              | (``-f``)                                                 |
   +------------------------------+----------------------------------------------------------+
   | -e                           | The relative path of a                                   |
   |                              | single element in a                                      |
   | -\\-element                  | repository, relative to                                  |
   |                              | its future position in                                   |
   |                              | the repository.                                          |
   |                              |                                                          |
   |                              | E.g.                                                     |
   |                              | ``/databases/admin/view                                  |
   |                              | s/incidents.vql``                                        |
   +------------------------------+----------------------------------------------------------+
   | -re                          | The identifier path of                                   |
   |                              | an element.                                              |
   | -\\-repository-element       |                                                          |
   |                              | The syntax of this                                       |
   |                              | identifier is:                                           |
   |                              | ``database:type[:subtype]:[folderPath/]name``            |
   |                              |                                                          |
   |                              | E.g.                                                     |
   |                              | admin:view:/incidents                                    |
   |                              |                                                          |
   |                              | The values of ``type``                                   |
   |                              | are ``datasource``,                                      |
   |                              | ``view``,                                                |
   |                              | ``webservice``,                                          |
   |                              | ``widget``,                                              |
   |                              | ``storedprocedure``,                                     |
   |                              | ``folder``,                                              |
   |                              | ``database``.                                            |
   |                              |                                                          |
   |                              | The value of                                             |
   |                              | ``subtype`` can be                                       |
   |                              | ``arn``, ``custom``,                                     |
   |                              | ``df``, ``essbase``,                                     |
   |                              | ``gs``, ``itp``,                                         |
   |                              | ``jdbc``, ``json``,                                      |
   |                              | ``ldap``, ``odbc``,                                      |
   |                              | ``olap``, ``salesforce``, ``sapbwbapi``,                 |
   |                              | ``saperp``, ``ws``,                                      |
   |                              | ``xml``.                                                 |
   +------------------------------+----------------------------------------------------------+
   | -su                          | Only valid when                                          |
   |                              | exporting to a file                                      |
   | -\\-singleuser               | (``--file``)                                             |
   |                              |                                                          |
   |                              | If present, the file                                     |
   |                              | generated will begin                                     |
   |                              | with the statement                                       |
   |                              | ``ENTER SINGLE USER MODE``                               |
   |                              | and end with                                             |
   |                              | ``EXIT SINGLE USER MODE``,                               |
   |                              |                                                          |
   |                              | so that Virtual                                          |
   |                              | DataPort switches to                                     |
   |                              | single user mode while                                   |
   |                              | importing this file.                                     |
   |                              |                                                          |
   |                              | This option is ignored                                   |
   |                              | when exporting to a                                      |
   |                              | Repository.                                              |
   |                              |                                                          |
   |                              | **Note**: you are                                        |
   |                              | strongly advised to                                      |
   |                              | import metadata into a                                   |
   |                              | Virtual DataPort server                                  |
   |                              | in single user mode.                                     |
   +------------------------------+----------------------------------------------------------+
   | -P                           | Set of settings that                                     |
   |                              | control the export                                       |
   | -\\-property                 | process. You can                                         |
   |                              | specify one or more                                      |
   |                              | properties with this                                     |
   |                              | syntax:                                                  |
   |                              |                                                          |
   |                              | ``<property name> = {yes | no}``                         |
   |                              |                                                          |
   |                              | `Properties than can be                                  |
   |                              | passed to the export                                     |
   |                              | script (--property                                       |
   |                              | parameter)`_ lists the                                   |
   |                              | available properties.                                    |
   +------------------------------+----------------------------------------------------------+

.. todo: Review this table to avoid that it looks weird.

The properties that control the export process (``-P``, ``--property``)
are the following:

.. table:: Properties than can be passed to the export script (--property parameter)
   :name: Properties than can be passed to the export script (--property parameter)

   +--------------------------------------+------------------------------------------------------------+
   | cluster                              | Default value: ``no``.                                     |
   |                                      |                                                            |
   |                                      | If ``yes``, the following elements                         |
   |                                      | will *not* be exported:                                    |
   |                                      |                                                            |
   |                                      | -  JMS listeners: only one of the                          |
   |                                      |    Servers must connect to a JMS queue                     |
   |                                      |    or topic.                                               |
   |                                      |                                                            |
   |                                      | -  The configuration of the "Cache                         |
   |                                      |    maintenance task" of each database.                     |
   |                                      |    However, even with this option, it                      |
   |                                      |    does export the global configuration                    |
   |                                      |    of the "Cache maintenance task".                        |
   |                                      |                                                            |
   |                                      | -  Server connectivity settings:                           |
   |                                      |                                                            |
   |                                      |    -  Server port number                                   |
   |                                      |    -  Shutdown port number                                 |
   |                                      |    -  Auxiliary port number                                |
   |                                      |    -  ODBC port number                                     |
   |                                      |    -  Web container HTTP port number                       |
   |                                      |    -  Web container shutdown port                          |
   |                                      |       number                                               |
   |                                      |    -  Auxiliary Web container port                         |
   |                                      |       number                                               |
   |                                      |                                                            |
   |                                      | -  JVM settings of the Server and the                      |
   |                                      |    embedded Web container                                  |
   |                                      |                                                            |
   |                                      |                                                            |
   |                                      | When working in an environment with                        |
   |                                      | a cluster of Virtual DataPort                              |
   |                                      | servers. The main Server must have                         |
   |                                      | different settings from the rest.                          |
   |                                      | E.g., the "Cache maintenance task"                         |
   |                                      | only can be active in one of the                           |
   |                                      | Servers. In this scenario, we                              |
   |                                      | recommend adding the parameter                             |
   |                                      | ``-P cluster=yes`` when exporting the                      |
   |                                      | metadata from the main Server to import                    |
   |                                      | it into the other Server.                                  |
   |                                      |                                                            |
   |                                      | Adding the parameter ``-P``                                |
   |                                      | ``cluster=yes`` is equivalent to                           |
   |                                      | adding the following properties:                           |
   |                                      |                                                            |
   |                                      | -P includeJMSListeners=no                                  |
   |                                      |                                                            |
   |                                      | -P includeDatabaseCacheMaintenanceProperties=no            |
   |                                      |                                                            |
   |                                      | .. code-block:: none                                       |
   |                                      |                                                            |
   |                                      |    -P                                                      |
   |                                      |    excludeServerConfigurationProperties                    |
   |                                      |    =com.denodo.vdb.vdbinterface.server.                    |
   |                                      |    VDBManagerImpl.registryURL,com.denod\                   |
   |                                      |    o.vdb.vdbinterface.server.VDBManager\                   |
   |                                      |    Impl.registryPort,com.denodo.vdb.vdb\                   |
   |                                      |    interface.server.VDBManagerImpl.shut\                   |
   |                                      |    downPort,com.denodo.vdb.vdbinterface\                   |
   |                                      |    .server.VDBManagerImpl.factoryPort,c\                   |
   |                                      |    om.denodo.vdb.vdbinterface.server.VD\                   |
   |                                      |    BManagerImpl.odbcPort,com.denodo.vdb\                   |
   |                                      |    .cache.cacheMaintenance,java.env.DEN\                   |
   |                                      |    ODO\_OPTS\_START,java.env.DENODO\_OPTS\_STOP            |
   |                                      |                                                            |
   |                                      |    -P                                                      |
   |                                      |    excludeWebContainerConfigurationProp\                   |
   |                                      |    erties=com.denodo.tomcat.http.port,c\                   |
   |                                      |    om.denodo.tomcat.shutdown.port,com.d\                   |
   |                                      |    enodo.tomcat.jmx.rmi.port,com.denodo\                   |
   |                                      |    .tomcat.jmx.port,java.env.DENODO\_OP\                   |
   |                                      |    TS\_START,java.env.DENODO\_OPTS\_STOP                   |
   +--------------------------------------+------------------------------------------------------------+
   | exclude_jdbc_wrapper_properties      | When you add ``-P includeProperties=yes``, add             |
   |                                      | ``-P exclude_jdbc_wrapper_properties=yes`` if you want the |
   |                                      | parameters ``CATALOGNAME`` and ``SCHEMANAME`` of the       |
   |                                      | ``CREATE WRAPPER JDBC`` statements to contain the actual   |
   |                                      | value instead of a property.                               |
   |                                      |                                                            |
   |                                      | Default value: ``no``.                                     |
   +--------------------------------------+------------------------------------------------------------+
   | excludeServerConfigurationProperties | When exporting the entire Virtual                          |
   |                                      | DataPort server, the output includes                       |
   |                                      | all the settings of the Server:                            |
   |                                      | cache module, concurrent requests,                         |
   |                                      | default i18n, etc.                                         |
   |                                      |                                                            |
   |                                      | This property is a comma-separated                         |
   |                                      | list of these properties whose value                       |
   |                                      | will not be exported. Make sure you                        |
   |                                      | do not put the space character                             |
   |                                      | between the comma and the name of                          |
   |                                      | each property.                                             |
   |                                      |                                                            |
   |                                      | Default value: empty list.                                 |
   |                                      |                                                            |
   |                                      | Valid values:                                              |
   |                                      |                                                            |
   |                                      | -  ``com.denodo.vdb.vdbinterface.ser                       |
   |                                      |    ver.VDBManagerImpl.registryURL``                        |
   |                                      | -  ``com.denodo.vdb.vdbinterface.ser                       |
   |                                      |    ver.VDBManagerImpl.registryPort``                       |
   |                                      | -  ``com.denodo.vdb.vdbinterface.ser                       |
   |                                      |    ver.VDBManagerImpl.shutdownPort``                       |
   |                                      | -  ``com.denodo.vdb.vdbinterface.ser                       |
   |                                      |    ver.VDBManagerImpl.factoryPort``                        |
   |                                      | -  ``com.denodo.vdb.vdbinterface.ser                       |
   |                                      |    ver.VDBManagerImpl.odbcPort``                           |
   |                                      | -  ``com.denodo.vdb.cache.cacheMaint                       |
   |                                      |    enance``                                                |
   |                                      | -  ``java.env.DENODO_OPTS_START``                          |
   |                                      | -  ``java.env.DENODO_OPTS_STOP``                           |
   +--------------------------------------+------------------------------------------------------------+
   | excludeWebContainerConfigurationProp | When exporting the entire Virtual                          |
   | erties                               | DataPort server, the output includes                       |
   |                                      | all the settings of the embedded Web                       |
   |                                      | container.                                                 |
   |                                      |                                                            |
   |                                      | This property is a comma-separated                         |
   |                                      | list of these properties, whose                            |
   |                                      | value will not be exported.                                |
   |                                      |                                                            |
   |                                      | Default value: empty list.                                 |
   |                                      |                                                            |
   |                                      | Valid values:                                              |
   |                                      |                                                            |
   |                                      | -  ``com.denodo.vdb.vdbinterface.ser                       |
   |                                      |    ver.VDBManagerImpl.registryURL``                        |
   |                                      | -  ``com.denodo.tomcat.http.port``                         |
   |                                      | -  ``com.denodo.tomcat.shutdown.port``                     |
   |                                      | -  ``com.denodo.tomcat.jmx.rmi.port``                      |
   |                                      | -  ``com.denodo.tomcat.jmx.port``                          |
   |                                      | -  ``java.env.DENODO_OPTS_START``                          |
   |                                      | -  ``java.env.DENODO_OPTS_STOP``                           |
   +--------------------------------------+------------------------------------------------------------+
   | includeEnvSpecificElements           | If ``yes``, the output does not                            |
   |                                      | include the VQL to create the                              |
   |                                      | elements that are independent of the                       |
   |                                      | environment and only includes the                          |
   |                                      | VQL to create the elements that                            |
   |                                      | depend on the environment such as                          |
   |                                      | data sources, users and their                              |
   |                                      | privileges, JMS listeners, Server                          |
   |                                      | settings, etc.                                             |
   |                                      |                                                            |
   |                                      | The section :ref:`Exporting                                |
   |                                      | Environment-Dependent and                                  |
   |                                      | Independent Elements to Different                          |
   |                                      | Files` lists which elements are                            |
   |                                      | considered dependent on the                                |
   |                                      | environment.                                               |
   |                                      |                                                            |
   |                                      | **Note**: this option is deprecated.                       |
   |                                      | Instead, use the option                                    |
   |                                      | ``includeProperties``.                                     |
   +--------------------------------------+------------------------------------------------------------+
   | includeNonEnvSpecificElements        | If ``yes``, the output does not                            |
   |                                      | include the VQL to create the                              |
   |                                      | elements that depend on the                                |
   |                                      | environment and only includes the                          |
   |                                      | VQL to create the elements that are                        |
   |                                      | independent of the environment such                        |
   |                                      | as views, Web services, widgets,                           |
   |                                      | etc.                                                       |
   |                                      |                                                            |
   |                                      | The section :ref:`Exporting                                |
   |                                      | Environment-Dependent and                                  |
   |                                      | Independent Elements to Different                          |
   |                                      | Files` lists which elements are                            |
   |                                      | considered independent from the                            |
   |                                      | environment.                                               |
   |                                      |                                                            |
   |                                      | **Note**: this option is deprecated.                       |
   |                                      | Instead, use the option                                    |
   |                                      | ``includeProperties``.                                     |
   +--------------------------------------+------------------------------------------------------------+
   | replaceExistingElements              | If ``yes``, the CREATE statements                          |
   |                                      | will have the REPLACE clause. E.g.:                        |
   |                                      |                                                            |
   |                                      | CREATE OR REPLACE VIEW p\_customer                         |
   |                                      | AS SELECT ...                                              |
   +--------------------------------------+------------------------------------------------------------+
   | dropElements                         | If ``yes``, the ``CREATE`` statement                       |
   |                                      | of each element are preceded by a                          |
   |                                      | ``DROP`` statement. E.g.:                                  |
   |                                      |                                                            |
   |                                      | ``DROP VIEW IF EXISTS p_customer CASCADE;                  |
   |                                      | CREATE VIEW p_customer AS SELECT ...``                     |
   +--------------------------------------+------------------------------------------------------------+
   | includeContents                      | If ``yes``, when exporting a folder,                       |
   |                                      | the output will have the VQL statements to create the      |      
   |                                      | folder and the elements contained in this folder,          |
   |                                      | including other folders.                                   |
   |                                      |                                                            |
   |                                      | If no, the output will only contain the VQL statement to   |
   |                                      | create the folder.                                         |
   |                                      |                                                            |
   |                                      | Default value: ``no``.                                     |
   +--------------------------------------+------------------------------------------------------------+
   | includeCreateDatabase                | Only valid when exporting an entire                        |
   |                                      | database.                                                  |
   |                                      |                                                            |
   |                                      | If ``yes``, the output includes the                        |
   |                                      | following statements that will                             |
   |                                      | recreate the exported database:                            |
   |                                      |                                                            |
   |                                      |   DROP DATABASE IF EXISTS <exported                        |
   |                                      |   db name>;                                                |
   |                                      |   CREATE DATABASE <exported db                             |
   |                                      |   name>;                                                   |
   |                                      |                                                            |
   |                                      | If you export all the metadata of a                        |
   |                                      | Server, the output already includes                        |
   |                                      | the statements to create the                               |
   |                                      | databases.                                                 |
   +--------------------------------------+------------------------------------------------------------+
   | includeCustomComponents              | If ``yes`` and the output includes                         |
   |                                      | any ITPilot wrappers, the output                           |
   |                                      | also includes the ITPilot Custom                           |
   |                                      | components used by these wrappers.                         |
   +--------------------------------------+------------------------------------------------------------+
   | includeDatabaseCacheMaintenanceProp\ | When exporting the metadata of the                         |
   | erties                               | Server or a single database, the                           |
   |                                      | output includes the settings of the                        |
   |                                      | "Cache maintenance task".                                  |
   |                                      |                                                            |
   |                                      | If ``no``, the output will *not*                           |
   |                                      | include the settings of the "Cache                         |
   |                                      | maintenance task" of any database.                         |
   |                                      | However, if you export the metadata                        |
   |                                      | of the entire Server, the output                           |
   |                                      | will include the global settings of                        |
   |                                      | this task.                                                 |
   |                                      |                                                            |
   |                                      | Default value: ``yes``.                                    |
   +--------------------------------------+------------------------------------------------------------+
   | includeDependencies                  | If ``yes``, the output includes the                        |
   |                                      | VQL statements to create the                               |
   |                                      | elements that the element you are                          |
   |                                      | exporting depends on.                                      |
   |                                      |                                                            |
   |                                      | For example, if you export a base                          |
   |                                      | view and set this option to ``yes``,                       |
   |                                      | the script will also export the                            |
   |                                      | metadata of the data source.                               |
   |                                      |                                                            |
   |                                      | Only useful when you are not                               |
   |                                      | exporting the metadata of an entire                        |
   |                                      | database or Server.                                        |
   +--------------------------------------+------------------------------------------------------------+
   | includeDeployments                   | If ``yes`` the output will include                         |
   |                                      | ``DEPLOY`` statements for the web                          |
   |                                      | services that are deployed.                                |
   |                                      |                                                            |
   |                                      | Default value: ``no``.                                     |
   +--------------------------------------+------------------------------------------------------------+
   | includeJars                          | If ``yes``, the output includes the                        |
   |                                      | Jars that were imported into the                           |
   |                                      | Server with the File > Jar                                 |
   |                                      | Management wizard of the                                   |
   |                                      | Administration Tool.                                       |
   |                                      |                                                            |
   |                                      | If you are not exporting the                               |
   |                                      | metadata of the entire Server, the                         |
   |                                      | output will only include the Jars                          |
   |                                      | used by the elements you are                               |
   |                                      | exporting.                                                 |
   |                                      |                                                            |
   |                                      | Default value: ``no``.                                     |
   +--------------------------------------+------------------------------------------------------------+
   | includeJMSListeners                  | If ``yes``, the output will include                        |
   |                                      | the statements to create the JMS                           |
   |                                      | listeners.                                                 |
   |                                      |                                                            |
   |                                      | Default value: ``yes``.                                    |
   +--------------------------------------+------------------------------------------------------------+
   | includeProperties                    | Generates two files: one that                              |
   |                                      | contains the metadata and another                          |
   |                                      | one that contains the values of the                        |
   |                                      | parameters whose value depend on the                       |
   |                                      | environment where the Server is                            |
   |                                      | running.                                                   |
   |                                      |                                                            |
   |                                      | This option cannot be used when                            |
   |                                      | exporting to a repository                                  |
   |                                      | (``--repository``)                                         |
   +--------------------------------------+------------------------------------------------------------+
   | includeScanners                      | If ``yes`` and the output includes                         |
   |                                      | any ITPilot wrappers, the output                           |
   |                                      | also includes the ITPilot scanners                         |
   |                                      | used by those wrappers.                                    |
   +--------------------------------------+------------------------------------------------------------+
   | includeStatistics                    | By default, the statistics of views are only exported when |
   |                                      | you export the entire server, not when you export a        |
   |                                      | database nor when you export a view. To include the        |
   |                                      | statistics of the views in the result, add the option      |
   |                                      | ``-P includeStatistics=yes``.                              |
   |                                      |                                                            |
   |                                      | Default value: ``no``.                                     |
   +--------------------------------------+------------------------------------------------------------+
   | includeUserPrivileges                | If yes, the output will include the                        |
   |                                      | statements to replicate the                                |
   |                                      | privileges of the exported elements.                       |
   |                                      | That is, it will include the                               |
   |                                      | statements to do the following:                            |
   |                                      |                                                            |
   |                                      | -  Create the users and the roles                          |
   |                                      |    that are owners or have                                 |
   |                                      |    privileges assigned over the                            |
   |                                      |    exported elements.                                      |
   |                                      | -  Assign the privileges to these                          |
   |                                      |    users/roles over the exported                           |
   |                                      |    views.                                                  |
   |                                      |                                                            |
   |                                      | Default value: ``no``.                                     |
   +--------------------------------------+------------------------------------------------------------+
   | includeVCSConfiguration              | When you export a database with the option                 |
   |                                      | ``-P includeCreateDatabase=yes``, the output contains the  |
   |                                      | statements CREATE DATABASE and ALTER DATABASE so the       |
   |                                      | database is created with the same configuration, including |
   |                                      | the VCS settings of the database. If you do not want       |
   |                                      | to export the VCS settings, add                            |
   |                                      | ``-P includeVCSConfiguration=no``.                         |
   |                                      |                                                            |
   |                                      | Default value: ``yes``.                                    |
   +--------------------------------------+------------------------------------------------------------+

..

   This script returns one of these exit codes:

   -  0: no errors. The metadata was exported successfully.
   -  126: one of the elements could not be exported.
   -  1: for other errors. E.g. the script could not open a connection to the Virtual DataPort Server.


.. rubric:: Examples

.. rubric:: Example 1:

.. code-block:: bash

   export --server "//localhost:9999/database1"
          --login admin
          --password encrypted:Gr16MjvuXhRzPtPH/yTXHw==
          --singleuser
          --file serverexport.vql
          --property includejars=yes
          --property includescanners=yes
          --property includeCustomComponents=yes

In this example, we export the metadata of the ``database1`` of the
Server running in ``localhost``, port ``9999``, using the credentials
``admin``/``admin``. The encrypted password has been obtained with the
script ``encrypt_password``.

The result is stored in the file ``serverexport.vql``.

.. rubric:: Example 2:

.. code-block:: bash

   export --server "//localhost:9999"
          --login admin
          --password admin
          --singleuser
          --repository "C:/repository"
          --property includejars=yes
          --property includescanners=yes
          --property includeCustomComponents=yes

In this example, we export to a Repository the metadata of all the
databases and the settings of the Server. The Repository will be created
in the directory ``C:\repository``

.. rubric:: Example 3:

.. code-block:: bash

   export --server "//localhost:9999"
          --login admin
          --password encrypted:Gr16MjvuXhRzPtPH/yTXHw==
          --singleuser
          --repository "C:/repository"
          --repository-element
            "vdp_testing:datasource:jdbc:/Data sources/internet_ds"


In this example, we export to a Repository the metadata of the data
source JDBC named ``internet_ds`` of the database ``vdp_testing``.
