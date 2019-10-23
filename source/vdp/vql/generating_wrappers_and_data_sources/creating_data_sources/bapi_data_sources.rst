=================
BAPI Data Sources
=================

Virtual DataPort can invoke SAP BAPIs (Business Application Programming
Interfaces) to obtain data stored in SAP ERP and other SAP applications.

To create a BAPI data source, use the statement CREATE DATASOURCE SAPERP.

.. important:: Before creating any BAPI data source, we have to install
   the SAP Java Connector 3 in the system where Virtual DataPort is
   running. The appendix :doc:`/platform/installation/postinstallation_tasks/postinstallation_tasks_in_virtual_dataport/installing_the_sap_jco_connector` of the
   Installation Guide explains how to do this.

.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE SAPERP statement (BAPI)
   :name: Syntax of the CREATE DATASOURCE SAPERP statement (BAPI)

   CREATE [ OR REPLACE ] DATASOURCE SAPERP <name:identifier>
       [ FOLDER = <literal> ]
       SYSTEMNAME = <literal>
       [ SAPROUTER = <literal> ]
       [ LANGUAGE = <literal> ]
       <connection type>
       CLIENTID = <literal>     
       {
           <credentials>
         | WITH PASS-THROUGH SESSION CREDENTIALS ( <credentials> )
       }
       [
           SNC_LIBRARY = <literal>
           SNC_PARTNER_NAME = <literal>
           SNC_QOP = <integer>
       ]
       [
           MAX_ACTIVE = <integer> 
           MAX_IDLE = <integer> 
           MAX_WAIT = <integer> 
           MIN_EVICTABLE_IDLE_TIME = <integer> 
           TIME_BETWEEN_EVICTION = <integer>
       ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]

   <credentials> ::= 
     USERNAME = <literal> USERPASSWORD = <literal> [ ENCRYPTED ]
   
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


To modify a BAPI data source, use the statement ALTER DATASOURCE SAPERP.

.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE SAPERP statement (BAPI)
   :name: Syntax of the ALTER DATASOURCE SAPERP statement (BAPI)

   ALTER DATASOURCE SAPERP <name:identifier>
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
       [
           SNC_LIBRARY = <literal>
           SNC_PARTNER_NAME = <literal>
           SNC_QOP = <integer>
       ]
       [
           MAX_ACTIVE = <integer> 
           MAX_IDLE = <integer> 
           MAX_WAIT = <integer> 
           MIN_EVICTABLE_IDLE_TIME = <integer> 
           TIME_BETWEEN_EVICTION = <integer>
       ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]
   
..

   <credentials> ::= (see :ref:`Syntax of the CREATE DATASOURCE SAPERP statement (BAPI)`)


All the parameters of these two commands refer to the connection details
to the SAP instance. The meaning of these parameters is explained in
more detail in the section :ref:`BAPI Sources` of the Administration Guide.


-  ``SYSTEMNAME``: The SAP system ID of SAP ERP.

-  ``SAPROUTER``. Route between the SAP routers and the target server.

-  ``USERNAME``. The user name used for access the data source.

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

-  ``USERPASSWORD``. The password of the user. The ``ENCRYPTED`` modifier
   indicates that the provided password is encrypted (this option is
   typically used by the Denodo export/import process only).

   The modifier ``WITH PASS-THROUGH SESSION CREDENTIALS`` means that when a
   user queries a view that uses a data source with this option, Virtual
   DataPort uses the credentials of this user to connect to the database.
   With this modifier, the values of the parameters ``USERNAME`` and
   ``PASSWORD`` are used only when creating a base view from this data
   source using the Administration Tool. I.e. to connect to SAP to obtain
   information about the SAP BAPI of the base view.
   
   If you created a data source with this option, but you want to
   query a view of this data source with other credentials than the ones
   you have used to connect to the Server, add the parameters ``USERNAME``
   and ``PASSWORD`` to the ``CONTEXT``. These two parameters are *only*
   taken into account when the data source has been created with the option
   ``WITH PASS-THROUGH SESSION CREDENTIALS``.
   
   For example, if ``view1`` has been created over a ``SAPERP`` data
   source with the option ``WITH PASS-THROUGH SESSION CREDENTIALS`` and
   you execute 
   
   .. code-block:: sql

      SELECT * FROM view1   
      CONTEXT(USERNAME = 'admin', PASSWORD = 'd4GvpKA5BiwoGUFrnH92DNq5TTNKWw58I86PVH2tQIs/q1RH9CkCoJj57NnQUlmvgvvVnBvlaH8NFSDM0x5fWCJiAvyia70oxiUWbToKkHl3ztgH1hZLcQiqkpXT/oYd' ENCRYPTED)
      
   the Server will connect to the data source of the view with the
   username "admin" and the password "password", ignoring the credentials
   used by the user to connect to the Server.
   
   It is mandatory to add the token ``ENCRYPTED`` and enter the password encrypted. To encrypt the password, use the command ``ENCRYPT_PASSWORD``. For example:
   
   .. code-block:: vql
   
      ENCRYPT_PASSWORD 'my_secret_password';
   
   When the data source is created with this option, the Server creates a
   pool for each pair user/password. Initially, these pools only have one
   connection (``initSize``) to prevent the creation of a lot of
   connections. The maximum number of connections for each one of these
   pool is the value of the parameter ``MAXACTIVE``.
   
   .. warning:: Users should be careful when enabling the cache for views
      that involve data sources with pass-through credentials enabled. The
      section “Considerations When Configuring Data Sources with Pass-Through
      Credentials” explains the issues that may arise.


Secure Network Communications (SNC) provides stronger authentication and
encryption mechanisms than the default security options of SAP. To
enable SNC between the Virtual DataPort server and SAP, add the
following parameters:

-  ``SNC_LIBRARY``: corresponds, on the Administration Tool, with the
   field “SAP Cryptographic library” of the “Advanced tab” of the data
   source configuration.
-  ``SNC_PARTNER_NAME``: corresponds with the field “Partner name”.
-  ``SNC_QOP``: corresponds with the field “Security level”. The values
   of this parameter can be one these:

.. table:: BAPI data sources: values of the parameter SNC\_QOP
   :name: BAPI data sources: values of the parameter SNC\_QOP

   +--------------------------------+--------------------------------------------+
   | Value of the SNC_QOP Parameter |  Security Level                            |
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


Clauses to configure the pool of connections of the data source to SAP
ERP:

-  ``MAX_ACTIVE``: maximum number of active connections in the pool.
-  ``MAX_IDLE``: maximum number of idle connections in the pool.
-  ``MAX_WAIT`` (milliseconds): maximum time a thread will wait to
   obtain a connection from the pool. When a query reaches this limit,
   the query that is requesting the connection will fail.
-  ``MIN_EVICTABLE_IDLE_TIME`` (milliseconds): minimum amount of time
   that a connection sits idle in the pool before it is eligible to be
   closed and removed from the pool.
-  ``TIME_BETWEEN_EVICTION`` (milliseconds): the Server examines
   periodically the idle connections of the pool to close them. This
   parameter indicates the minimum interval between these examinations.

|

-  ``TRANSFER_RATE_FACTOR``: relative measure of the speed of the network connection between the Denodo server and the data source. Use the default value (e.g. 1 for JDBC databases located on premises) if the data source is accessible through a conventional 100 Mbps LAN. Use higher values for faster networks and lower values for data sources accessible through a WAN.
   
   The cost optimizer uses this value when evaluating the cost of an execution plan. The default value is usually correct so you should not specify this parameter unless you have a deep knowledge of the cost optimizer. 
