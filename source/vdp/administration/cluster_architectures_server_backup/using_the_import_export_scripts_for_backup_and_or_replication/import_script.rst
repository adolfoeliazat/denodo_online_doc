=============
Import Script
=============

The ``import`` script imports the contents of a VQL file or a Repository
into a Virtual DataPort server or into several Virtual DataPort servers
at the same time.

The following sections explain how to:

-  Import the metadata from a VQL file or from a VQL file with a
   properties file.
   See section :ref:`Importing the Metadata from a Single VQL File`.
-  Import the metadata from a Repository.
   See section :ref:`Importing the Metadata from a Repository`.

This script is located in the directory :file:`{<DENODO_HOME>}/bin`.

This script returns one of these exit codes:

-  0: no errors. All the VQL statements in the file were executed successfully.
-  126: the execution of one or more VQL statements failed.
-  1: for other errors. E.g. the script could not open a connection to the Virtual DataPort Server.


Importing the Metadata from a Single VQL File
=================================================================================

Use the following syntax to import the metadata of a VQL file into one
or more Servers:

.. code-block:: bnf
   :caption: Syntax of the import script (importing from a single VQL file)
   :name: Syntax of the import script (importing from a single VQL file)
 
   import --file <input filename> 
        [ --properties-file <properties file name>]
        {  [ --server <server URI> 
          |  --servers-file <servers file> 
        }
        [ --singleuser ]
   
   <server URI> = 
   host:port/database?user@password[&queryTimeout=value][&chunkTimeout=value][&chunkSize=value]

.. todo: Review this table to avoid that it looks weird.


.. table:: Parameters of the import script when importing from a single VQL file
   :name: Parameters of the import script when importing from a single VQL file

   +-------------------------+---------------------------------------------------+
   | Parameter Name          | Description                                       |
   +=========================+===================================================+
   | -h                      | URI of the Server.                                |
   |                         |                                                   |
   | -\\-server <server URI> | You can specify the                               |
   |                         | password of the Server                            |
   |                         | encrypted. To do this,                            |
   |                         | execute the script                                |
   |                         | ``encrypt_password`` to                           |
   |                         | obtain the encrypted                              |
   |                         | password. Then, in the                            |
   |                         | password with                                     |
   |                         | ``encrypted:``                                    |
   |                         |                                                   |
   |                         | E.g.                                              |
   |                         | ``"//localhost:9999/a                             |
   |                         | dmin?login@encrypted:Gr                           |
   |                         | 16MjvuXhRzPtPH/yTXHw                              |
   |                         | =="``                                             |
   |                         |                                                   |
   |                         | If you use this script                            |
   |                         | to execute queries that                           |
   |                         | may take a long time,                             |
   |                         | add the                                           |
   |                         | ``queryTimeout``                                  |
   |                         | parameter to the URI to                           |
   |                         | avoid reaching the                                |
   |                         | query timeout. E.g.                               |
   |                         |                                                   |
   |                         | ``"//localhost:9999/adm                           |
   |                         | in?login@encrypted:Gr16                           |
   |                         | MjvuXhRzPtPH/yTXHw==&qu                           |
   |                         | eryTimeout=11111"``                               |
   |                         |                                                   |
   |                         | The value of the                                  |
   |                         | parameter has to be                               |
   |                         | surrounded with quotes                            |
   |                         | so the operating system                           |
   |                         | identifies the value as                           |
   |                         | a single parameter.                               |
   |                         |                                                   |
   |                         | You can specify more                              |
   |                         | than one ``--server``                             |
   |                         | parameter and the                                 |
   |                         | metadata will be                                  |
   |                         | imported into all of                              |
   |                         | them.                                             |
   +-------------------------+---------------------------------------------------+
   | -s                      | Instead of using the parameter\ ``--server``,     |
   |                         | path to a file                                    |
   | -\\-servers-file        | containing URIs of                                |
   | <servers file>          | Virtual DataPort                                  |
   |                         | Servers.                                          |
   |                         |                                                   |
   |                         | The file                                          |
   |                         | ``<servers file>`` must                           |
   |                         | contain one or more                               |
   |                         | URIs. Each URI has to                             |
   |                         | be indicated in a                                 |
   |                         | separate line, with the                           |
   |                         | same syntax of the                                |
   |                         | parameter ``--server``.                           |
   +-------------------------+---------------------------------------------------+
   | -f                      | Path to the file that                             |
   |                         | contains the metadata                             |
   | -\\-file <input file>   | to be imported.                                   |
   |                         |                                                   |
   |                         | This parameter has a                              |
   |                         | different meaning if                              |
   |                         | the metadata is                                   |
   |                         | imported from a                                   |
   |                         | repository instead of                             |
   |                         | from a single file.                               |
   +-------------------------+---------------------------------------------------+
   | -pf                     | Path to the properties                            |
   |                         | file with the values of                           |
   | -\\-properties-file     | the parameters that                               |
   | <properties file>       | depend on the                                     |
   |                         | environment where the                             |
   |                         | Server is running.                                |
   |                         |                                                   |
   |                         | Use this option when                              |
   |                         | the metadata was                                  |
   |                         | exported with the                                 |
   |                         | option                                            |
   |                         | ``--property includeP                             |
   |                         | roperties``                                       |
   |                         | of the export script                              |
   |                         | or with “Include                                  |
   |                         | properties file” of                               |
   |                         | the Administration                                |
   |                         | Tool.                                             |
   +-------------------------+---------------------------------------------------+
   | -su                     | If present, the import                            |
   |                         | script will execute the                           |
   | -\\-singleuser          | statement                                         |
   |                         | ``ENTER SINGLE USER MODE``                        |
   |                         | before executing the                              |
   |                         | VQL statements of the                             |
   |                         | file. At the end, the                             |
   |                         | script will execute                               |
   |                         | ``EXIT SINGLE USER                                |
   |                         | MODE``.                                           |
   |                         |                                                   |
   |                         | By doing this, Virtual                            |
   |                         | DataPort switches to                              |
   |                         | single user mode while                            |
   |                         | importing the file.                               |
   |                         |                                                   |
   |                         | **Note**: you are                                 |
   |                         | strongly advised to                               |
   |                         | import metadata in a                              |
   |                         | Virtual DataPort server                           |
   |                         | in single user mode.                              |
   +-------------------------+-------------------------+-------------------------+


**Examples**

**Example 1**:

.. code-block:: bash

   import --singleuser --file export.vql 
          --server "localhost:9999/admin?admin@admin1" 
          --server "host2:9099/admin?admin@admin2"

In this example, the script imports the metadata of the file
``export.vql`` into two Servers.

**Example 2**:

.. code-block:: bash

   import --singleuser -f export.vql -s servers.conf

In this example, the script imports the metadata of the file
``export.vql`` and reads the URIs of the target Servers from the file
``servers.conf``.


Importing the Metadata from a Repository
=================================================================================

Use the following syntax to import the metadata of a Repository into one
or more Servers.

Besides importing the metadata of the Repository to a Server(s), the
script can *instead*, create a single file with all VQL statements of
the Repository. To do this, use the option ``--file``.

.. note:: If you want to store and import the metadata of the Server
   in/from a VCS, we recommend using the VCS integration of Virtual
   DataPort (see section :ref:`Version Control Systems Integration`).

 
.. code-block:: bash
   :name: Syntax of the import script (importing from a repository)
   :caption: Syntax of the import script (importing from a repository)

   import --repository <path to repository>
        [ {   --element <path to element> 
            | --repository-element <identifier path> } ]*
        [ --property <property name> = { yes | no } ]*
        {   --server <server URI> [--server <server URI>]*
          | --servers-file <servers file>
        }
        [ --file <output file> ] [ --singleuser ]




 
.. table:: Parameters of the import script when importing from a repository
   :name: Parameters of the import script when importing from a repository

   +-------------------------+---------------------------------------------------+
   | Parameter Name          | Description                                       |
   +=========================+===================================================+
   | -r                      | Path to the directory of the Repository.          |
   |                         |                                                   |
   | -\\-repository          |                                                   |
   +-------------------------+---------------------------------------------------+
   | -e                      | Path inside the                                   |
   |                         | repository of the                                 |
   | -\\-element             | element you want to                               |
   |                         | import. Use this option                           |
   |                         | if you only want to                               |
   |                         | import some elements of                           |
   |                         | the Repository.                                   |
   |                         |                                                   |
   |                         | E.g.                                              |
   |                         | ``/databases/admin/view                           |
   |                         | s/incidents.vql``                                 |
   |                         |                                                   |
   |                         | If you want to import                             |
   |                         | an element and its                                |
   |                         | dependencies, add the                             |
   |                         | property                                          |
   |                         | ``includeDependencies``.                          |
   |                         |                                                   |
   |                         | E.g.:                                             |
   |                         | ``--property includeDep                           |
   |                         | endencies=true``                                  |
   +-------------------------+---------------------------------------------------+
   | -re                     | The identifier path of                            |
   |                         | an element stored in                              |
   | -\\-repository-element  | the repository                                    |
   |                         |                                                   |
   |                         | Syntax:                                           |
   |                         | ``database:type[:subtyp                           |
   |                         | e]:[folderPath/]name``                            |
   |                         |                                                   |
   |                         | E.g.                                              |
   |                         | ``admin:view:/incidents                           |
   |                         | .vql``                                            |
   |                         |                                                   |
   |                         | The available types are                           |
   |                         | ``datasource``,                                   |
   |                         | ``view``,                                         |
   |                         | ``webservice``,                                   |
   |                         | ``widget``,                                       |
   |                         | ``storedprocedure``,                              |
   |                         | ``folder``,                                       |
   |                         | ``database``.                                     |
   |                         |                                                   |
   |                         | The value of subtype                              |
   |                         | can be ``arn``,                                   |
   |                         | ``custom``, ``df``,                               |
   |                         | ``essbase``, ``gs``,                              |
   |                         | ``itp``, ``jdbc``,                                |
   |                         | ``json``, ``ldap``,                               |
   |                         | ``odbc``, ``olap``,                               |
   |                         | ``salesforce``,                                   |
   |                         | ``sapbwbapi``,                                    |
   |                         | ``saperp``, ``ws``,                               |
   |                         | ``xml``.                                          |
   +-------------------------+---------------------------------------------------+
   | -h                      | URI of the Server.                                |
   |                         |                                                   |                         
   | -\\-server <host>:      | If you want to use an                             |
   | <port>/<database>       | encrypted password, use                           |
   | ?<user>@<password>      | the script                                        |
   |                         | ``encrypt_password`` to                           |
   |                         | obtain your password,                             |
   |                         | encrypted.                                        |
   |                         |                                                   |
   |                         | In this case, the value                           |
   |                         | of this parameter must                            |
   |                         | be prefixed with                                  |
   |                         | ``encrypted:``                                    |
   |                         |                                                   |
   |                         | E.g.:                                             |
   |                         |                                                   |
   |                         | //localhost:9999/admin?admin@encrypted:Gr16Mjv\   |
   |                         | uXhRzPtPH/yTXHw==                                 |
   +-------------------------+---------------------------------------------------+
   | -s                      | Instead of using the                              |
   |                         | parameter ``--server``,                           |
   | -\\-servers-file        | you can provide the                               |
   |                         | path to a file that                               |
   |                         | contains the URIs of                              |
   |                         | Servers to which the                              |
   |                         | metadata will be                                  |
   |                         | imported.                                         |
   |                         |                                                   |
   |                         | Each URI has to be                                |
   |                         | indicated in a separate                           |
   |                         | line, with the same                               |
   |                         | syntax of the parameter                           |
   |                         | ``--server``.                                     |
   +-------------------------+---------------------------------------------------+
   | -f                      | Path to the VQL file                              |
   |                         | that contains the                                 |
   | -\\-file <input file>   | metadata to be                                    |
   |                         | imported.                                         |
   +-------------------------+---------------------------------------------------+
   | -su                     | If present, the import                            |
   |                         | script will execute the                           |
   | -\\-singleuser          | statement                                         |
   |                         | ``ENTER SINGLE USER MODE``                        |
   |                         | before executing the                              |
   |                         | VQL statements of the                             |
   |                         | file. At the end, the                             |
   |                         | script will execute                               |
   |                         | ``EXIT SINGLE USER                                |
   |                         | MODE``.                                           |
   |                         |                                                   |
   |                         | By doing this, Virtual                            |
   |                         | DataPort switches to                              |
   |                         | single user mode while                            |
   |                         | importing the file.                               |
   |                         |                                                   |
   |                         | **Note**: you are                                 |
   |                         | strongly advised to                               |
   |                         | import metadata in a                              |
   |                         | Virtual DataPort server                           |
   |                         | in single user mode.                              |
   +-------------------------+---------------------------------------------------+
   | -P                      | Set of settings that                              |
   |                         | control the import                                |
   | -\\-property            | process. You can                                  |
   |                         | specify one or more                               |
   |                         | properties with this                              |
   |                         | syntax:                                           |
   |                         |                                                   |
   |                         | <property name> = {yes \| no}                     |
   |                         |                                                   |
   |                         | `Properties than can be                           |
   |                         | passed to the import                              |
   |                         | script (--property                                |
   |                         | parameter): importing                             |
   |                         | from a repository`_                               |
   |                         | lists the available                               |
   |                         | properties.                                       |
   +-------------------------+---------------------------------------------------+


The properties that control the import process (``-P``, ``--property``)
are the following:

.. table:: Properties than can be passed to the import script (``--property`` parameter): importing from a repository
   :name: Properties than can be passed to the import script (--property parameter): importing from a repository
      
   +--------------------------------------+--------------------------------------+
   | Property as argument of --property   | Description                          |
   +======================================+======================================+
   | includeCreateDatabase                | Only valid when importing an entire  |
   |                                      | database and not when importing an   |
   |                                      | entire repository or just some       |
   |                                      | elements of it.                      |
   |                                      |                                      |
   |                                      | If ``yes``, the script executes the  |
   |                                      | statements that recreate the         |
   |                                      | exported database and restore its    |
   |                                      | configuration. For example:          |
   |                                      |                                      |
   |                                      | | DROP DATABASE IF EXISTS <exported  |
   |                                      |   db name>;                          | 
   |                                      | | CREATE DATABASE <exported db       |
   |                                      |   name>                              |
   |                                      | | ALTER DATABASE <exported db name>  |
   |                                      |   CACHE OFF;                         |
   |                                      |                                      |
   |                                      | If you export all the metadata of a  |
   |                                      | Server, the output already includes  |
   |                                      | the statements to create the         |
   |                                      | databases.                           |
   |                                      |                                      |
   |                                      | Default value: ``no``.               |
   +--------------------------------------+--------------------------------------+
   | includeDependencies                  | If ``yes``, the script imports the   |
   |                                      | elements of the parameters selected  |
   |                                      | with the parameters                  |
   |                                      |                                      |
   |                                      | ``--element`` or                     |
   |                                      | ``--repository-element`` *and also   |
   |                                      | imports their dependencies* (except  |
   |                                      | jars, scanners and custom            |
   |                                      | components).                         |
   |                                      |                                      |
   |                                      | For example, if the value of this    |
   |                                      | parameter is ``yes`` and you are     |
   |                                      | importing a derived view, the script |
   |                                      | also imports the base views and data |
   |                                      | sources that are required to create  |
   |                                      | this view.                           |
   |                                      |                                      |
   |                                      | Default value: ``no``.               |
   +--------------------------------------+--------------------------------------+
   | includeJars                          | If ``yes``, the script imports the   |
   |                                      | ``jar`` files that were exported     |
   |                                      | from the Server. Otherwise, they are |
   |                                      | not imported.                        |
   |                                      |                                      |
   |                                      | Default value: ``no``.               |
   +--------------------------------------+--------------------------------------+
   | includeEnvSpecificElements and       | These options have to be used        |
   | includeNonEnvSpecificElements        | together to:                         |
   |                                      |                                      |
   |                                      | -  Only import elements that depend  |
   |                                      |    on the environment such as data   |
   |                                      |    sources, users and their          |
   |                                      |    privileges, JMS listeners, Server |
   |                                      |    settings, etc.                    |
   |                                      | -  Or, only import elements that are |
   |                                      |    independent of the environment    |
   |                                      |    such as views, Web services, etc. |
   |                                      |                                      |
   |                                      | With the following options, the      |
   |                                      | script only imports the metadata of  |
   |                                      | elements dependent on the            |
   |                                      | environment:                         |
   |                                      |                                      |
   |                                      | -P includeNonEnvSpecificElements=no  |
   |                                      |                                      |
   |                                      | -P includeEnvSpecificElements=yes    |
   |                                      |                                      |
   |                                      | With the following options, the      |
   |                                      | script imports the metadata of the   |
   |                                      | elements that are independent of the |
   |                                      | environment:                         |
   |                                      |                                      |
   |                                      | -P includeNonEnvSpecificElements=yes |
   |                                      |                                      |
   |                                      | -P includeEnvSpecificElements=no     |
   |                                      |                                      |
   |                                      | The section :ref:`Exporting          |
   |                                      | Environment-Dependent and            |
   |                                      | Independent Elements to Different    |
   |                                      | Files` lists which elements are      |
   |                                      | considered dependent on the          |
   |                                      | environment and which are not.       |
   |                                      |                                      |
   |                                      | **Note**: these options are          |
   |                                      | deprecated and should not be used.   |
   |                                      | The option ``includeProperties``     |
   |                                      | should be used instead.              |
   +--------------------------------------+--------------------------------------+
   | includeDeployments                   | If ``yes``, the script imports the   |
   |                                      | deployment state of the Web Services |
   |                                      | that are being imported.             |
   |                                      |                                      |
   |                                      | Default value: ``no``.               |
   +--------------------------------------+--------------------------------------+


**Example 1**

.. code-block:: bash

   import --singleuser --repository "C:/repository" 
     --server localhost:9999/admin?admin@admin 
     --server acme:12999/testing?admin@encrypted:Gr16MjvuXhRzPtPH/yTXHw==

In this example, the script imports the content of the Repository
located in the path ``C:\repository``, into two Virtual DataPort
servers:

#. A server running in ``localhost``, port ``9999``, using the
   credentials ``admin/admin``.
#. A server running in the host acme. Note that the password is
   encrypted.

**Example 2**:

.. code-block:: bash

   import --singleuser --repository "C:/repository" 
          --servers-file servers.conf

In this example, the script reads the URIs of the target Servers from
the file ``servers.conf`` and imports the content of the repository into
these Servers.

**Example 3**:

.. code-block:: bash

   import --singleuser --repository "C:/repository" --file output.vql

In this example, the script creates a file ``output.vql`` with all the
VQL statements of the Repository. It does not import anything into any
Server. It just creates the file.

**Example 4**:

.. code-block:: bash

   import --singleuser --repository "c:\repository" --file output.vql 
          --element "databases\admin\folders\baseviews\views\bv_salesforce_lead.vql" 
          --dependencies --server localhost:13999/admin?admin@admin



In this example, the script executes the VQL statements contained in the
file ``databases\admin\folders\baseviews\views\bv_salesforce_lead.vql``.

The path of this file is relative to the path indicated in the
``--repository`` parameter.

By adding the parameter ``--dependencies``, the script will execute
first the dependencies of ``bv_salesforce_lead.vql``. To know the
dependencies of this element, the script reads the file
``bv_salesforce_lead.dependencies``, which contains the VQL files that
have to be processed to create the elements that
``bv_salesforce_lead.vql`` depends on.
