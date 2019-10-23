=============================
Multidimensional Data Sources
=============================

Virtual DataPort can obtain data from multidimensional databases such as
SAP BW3, SAP BI 7, Mondrian, Oracle Essbase and Microsoft SQL Server
Analysis Services.

There are four types of data sources to access the different types of
multidimensional databases:

-  ``ESSBASE``: it connects to Oracle Essbase databases. Before creating
   an ``ESSBASE`` data source, follow the steps described in the section :ref:`Installing the Connector for Oracle Essbase` of the Denodo Platform
   Installation Guide.
-  ``OLAP``: it connects to any multidimensional database that provides
   an XMLA interface. For example, Mondrian and Microsoft SQL Server
   Analysis Services.
-  ``SAPBWBAPI``: it connects to SAP using the SAP JCo connector. Before
   creating a ``SAPBWBAPI`` data source, follow the steps described in
   the section :ref:`Installing the SAP JCo Connector` of the Denodo Platform
   Installation Guide.

In the Administration Tool, all these data sources are grouped together
as if there was only one type of multidimensional data sources.

The data sources ``OLAP`` and ``SAPBWBAPI`` use a pool of connections to
manage the connections to the database.

The following figures contain the syntax of the commands to create and
modify data sources that connect to multidimensional databases:

-  SAPBWBAPI
-  ESSBASE
-  OLAP

Creating Multidimensional Data Sources with the SAP BAPI
========================================================

.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE SAPBWBAPI statement
   :name: Syntax of the CREATE DATASOURCE SAPBWBAPI statement

   CREATE [ OR REPLACE ] DATASOURCE SAPBWBAPI <name:identifier>
       [ FOLDER = <literal> ]
       DATABASENAME = <literal> 
       DATABASEVERSION = <literal>
       SYSTEMNAME = <literal>
       [ SAPROUTER = <literal> ]
       [ LANGUAGE = <literal> ]
       <connection type>
       CLIENTID = <literal>     
       {
           <credentials>
         | WITH PASS-THROUGH SESSION CREDENTIALS ( <credentials> )
       }
       READBLOCKSIZE = <integer> 
       [
           SNC_LIBRARY = <literal>
           SNC_PARTNER_NAME = <literal>
           SNC_QOP = <integer>
       ]
       [
           MAXACTIVE = <integer> 
           MAXIDLE = <integer> 
           MAXWAIT = <integer> 
           MINEVICTABLEIDLETIME = <integer> 
           TIMEBETWEENEVICTION = <integer>
       ] 
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]
       
   <connection type> ::=
         <direct connection>
       | <logon load balanced connection>
   
   <direct connection> ::=
       HOSTNAME = <literal>
       SYSTEMNUMBER = <literal>    
   
   <logon load balanced connection> ::=
       MESSAGE_HOST = <literal>
       [ MESSAGE_SERVICE = <literal> ]
       SID = <literal>
       [ GROUP = <literal> ]
   
   <credentials> ::= 
     USERNAME = <literal> USERPASSWORD = <literal> [ ENCRYPTED ]


.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE SAPBWBAPI statement
   :name: Syntax of the ALTER DATASOURCE SAPBWBAPI statement

   ALTER DATASOURCE SAPBWBAPI <name:identifier>
       [ DATABASENAME = <literal> ]
       [ DATABASEVERSION = <literal> ]
       [ SYSTEMNAME = <literal> ]
       [ HOSTNAME = <literal> ]
       [ MESSAGE_HOST = <literal> ]
       [ MESSAGE_SERVICE = <literal> ]
       [ SID = <literal> ]
       [ GROUP = <literal> ]
       [ CLIENTID = <literal> ]
       [ SYSTEMNUMBER = <literal> ]
       [ 
         <credentials>
         | WITH PASS-THROUGH SESSION CREDENTIALS ( <credentials> ) 
       ]
       [ READBLOCKSIZE = <integer> ]
       [
           SNC_LIBRARY = <literal>
           SNC_PARTNER_NAME = <literal>
           SNC_QOP = <integer>
       ]    
       [ MAXACTIVE = <integer> ]
       [ MAXIDLE = <integer> ]
       [ MAXWAIT = <integer> ]
       [ MINEVICTABLEIDLETIME = <integer> ]
       [ TIMEBETWEENEVICTION = <integer> ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]

   <credentials> ::= 
     USERNAME = <literal> USERPASSWORD = <literal> [ ENCRYPTED ]
   

-  ``SYSTEMNAME``. The SAP system ID of SAP ERP.

-  ``SAPROUTER``. Route between the SAP routers and the target server.

-  ``USERNAME``. The user name used to access SAP.

-  ``LANGUAGE``. Language of the connection established with the SAP
   server.

-  Connection type:


   -  If the connection type is “Direct”, provide:

      -  ``HOSTNAME``. Host where SAP is running.
      -  ``SYSTEMNUMBER``. Two-digit number that differentiates the SAP
         instances running on the same host.

   -  If the connection type is “Logon load balanced”, enter:

      -  ``MESSAGE_HOST``. Host of the SAP server that provides the data for
         choosing an appropriate application server.
      -  ``MESSAGE_SERVICE``. Port where the “SAP Message server” listens to
         connections.
      -  ``SID``. System ID of the SAP system.
      -  ``GROUP``. Name of the group of SAP application servers.

-  ``CLIENTID``. Identifier of the client.

-  ``USERNAME``: The user name used for access SAP.

-  ``USERPASSWORD``: The password of the user. You can provide the password
   in clear or encrypted. If you provide it encrypted, add the
   ``ENCRYPTED`` modifier after the password.

   To generate an encrypted password, execute the statement
   ``ENCRYPT_PASSWORD`` followed by the password. For example,
   ``ENCRYPT_PASSWORD 'password';``.
   
   The modifier ``WITH PASS-THROUGH SESSION CREDENTIALS`` means that when a
   user queries a view that uses a data source with this option, Virtual
   DataPort uses the credentials of this user to connect to SAP. With this
   modifier, the values of the parameters ``USERNAME`` and ``PASSWORD`` are
   used only by the Administration Tool to connect to the database and show
   the schemas of the database and their tables/views. But not for querying
   tables or views of the database.
   
   If you created the data source with this option, but you want to
   query a view of this data source with other credentials than the ones
   you have used to connect to the Server, add the parameters ``USERNAME``
   and ``USERPASSWORD`` to the ``CONTEXT``. These two parameters are *only*
   taken into account when the data source has been created with the option
   ``WITH PASS-THROUGH SESSION CREDENTIALS``.

   For example, if ``view1`` has been created over a data source with the
   option ``WITH PASS-THROUGH SESSION CREDENTIALS`` and you
   execute 
   
   .. code-block:: vql
   
      SELECT * 
      FROM view1 
      CONTEXT(USERNAME = 'admin', PASSWORD = 'd4GvpKA5BiwoGUFrnH92DNq5TTNKWw58I86PVH2tQIs/q1RH9CkCoJj57NnQUlmvgvvVnBvlaH8NFSDM0x5fWCJiAvyia70oxiUWbToKkHl3ztgH1hZLcQiqkpXT/oYd' ENCRYPTED)
      
   the Server will connect to the source with the username “admin” and the
   password “password”, ignoring the credentials used by the user to
   connect to the Server.
   
   It is mandatory to add the token ``ENCRYPTED`` and enter the password encrypted. To encrypt the password, execute the command ``ENCRYPT_PASSWORD``. For example:
   
   .. code-block:: vql
   
      ENCRYPT_PASSWORD 'my_secret_password';

   When the data source is created with this option, the Server creates a
   pool of connections for each pair user/password. Initially, these
   pools only have one connection (``initSize``) to prevent the creation
   of a lot of connections. The maximum number of connections for each
   one of these pools is the value of the parameter ``MAXACTIVE``.

   .. warning:: Be careful when enabling the cache for views that involve
      “Considerations When Configuring Data Sources with Pass-Through
      Credentials” explains the issues that may arise.

-  ``READBLOCKSIZE``: The BAPI adapter retrieves the data in blocks. This
   is the maximum size of these blocks.


Secure Network Communications (SNC) provides stronger authentication and
encryption mechanisms than the default security options of SAP. To
enable SNC between the Virtual DataPort server and SAP, add the
following parameters:

-  ``SNC_LIBRARY``: corresponds, on the Administration Tool, with the
   field “SAP Cryptographic library” of the “Advanced tab” of the
   configuration of multidimensional data sources that use one of the
   “SAP (BAPI)” adapters.
-  ``SNC_PARTNER_NAME``: corresponds with the field “Partner name”.
-  ``SNC_QOP``: corresponds with the field “Security level”. The values
   of this parameter can be one these:

.. table:: SAPBWBAPI data sources: values of the parameter SNC\_QOP
   :name: SAPBWBAPI data sources: values of the parameter SNC\_QOP  

   +--------------------------------+--------------------------------------------+
   | Value of the SNC_QOP Parameter | Security Level                             |
   +================================+============================================+
   | 1                              | Secure authentication only                 |
   +--------------------------------+--------------------------------------------+
   | 2                              | Data integrity protection                  |
   +--------------------------------+--------------------------------------------+
   | 3                              | Data privacy protection                    |
   +--------------------------------+--------------------------------------------+
   | 8                              | Use the value from snc/data_protection/use |
   +--------------------------------+--------------------------------------------+
   | 9                              | Use the value from snc/data_protection/max |
   +--------------------------------+--------------------------------------------+



Creating Multidimensional Data Sources with the Oracle
======================================================

.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE ESSBASE statement
   :name: Syntax of the CREATE DATASOURCE ESSBASE statement

   CREATE [ OR REPLACE ] DATASOURCE ESSBASE <name:identifier>
       [ FOLDER = <literal> ]
       DATABASEVERSION = <literal>
       URI = <literal>
       USERNAME = <literal>
       USERPASSWORD = <literal> [ ENCRYPTED ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]



.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE ESSBASE statement
   :name: Syntax of the ALTER DATASOURCE ESSBASE statement

   ALTER DATASOURCE ESSBASE <name:identifier>
       [ DATABASEVERSION = <literal> ]
       [ URI = <literal> ]
       [ USERNAME = <literal> ]
       [ USERPASSWORD = <literal> [ ENCRYPTED ] ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]


Creating Multidimensional Data Sources with the Generic
=======================================================

.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE OLAP statement
   :name: Syntax of the CREATE DATASOURCE OLAP statement

   CREATE [ OR REPLACE ] DATASOURCE OLAP <name:identifier>
       [ FOLDER = <literal> ]
       [ DATABASENAME = <literal> DATABASEVERSION = <literal> ]
       XMLAURI = <literal>
       USERNAME = <literal>
       USERPASSWORD = <literal> [ ENCRYPTED ]
       [ INITIALSIZE = <integer> MAXACTIVE = <integer> ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]



.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE OLAP statement
   :name: Syntax of the ALTER DATASOURCE OLAP statement

   ALTER DATASOURCE OLAP <name:identifier>
       [ DATABASENAME = <literal> ]
       [ DATABASEVERSION = <literal> ]
       [ XMLAURI = <literal> ]
       [ USERNAME = <literal> ]
       [ USERPASSWORD = <literal> [ENCRYPTED] ]
       [ INITIALSIZE = <integer> MAXACTIVE = <integer> ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]


Invalidating the Metadata Cache of SAP BAPI Data Sources
=================================================================================

When working with multidimensional data sources with the SAP BI (BAPI) or
SAP BW (BAPI) adapter (``SAPBWBAPI`` data source), you may want to
enable the SAP Metadata Cache (see section :ref:`SAP Metadata Cache` section
of the Administration Guide)

This cache is cleared when you restart the Virtual DataPort server. You
can also delete the contents of this cache at any time, without
restarting the Server. To do this, execute the following VQL statement.



.. code-block:: bnf
   :caption: Syntax of the command to invalidate the SAP BW metadata cache
   :name: Syntax of the command to invalidate the SAP BW metadata cache

   ALTER DATASOURCE SAPBWBAPI <data source name:identifier>
       LEVELSCACHE INVALIDATE
       [ 
           SCHEMANAME = <schemaname>
           CUBENAME = <cubename>
           DIMENSIONNAME = <dimensionname>
           HIERARCHYNAME = <hierarchyname>
           LEVELNAME = <levelname>
       ]


You provide the name of the data source because there is an SAP metadata
cache per data source.

If you only add the clause ``LEVELSCACHE INVALIDATE``, you delete the
cache of all the members of this data source.

If you add the other clauses, you only delete the cache of a particular
level of a hierarchy.