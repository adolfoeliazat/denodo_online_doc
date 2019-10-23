=============================
Data Sources for Web Services
=============================

The statement CREATE DATASOURCE WS creates data sources that retrieve data from SOAP web services.

.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE WS statement
   :name: Syntax of the CREATE DATASOURCE WS statement

   CREATE [ OR REPLACE ] DATASOURCE WS <name:identifier>
       [ FOLDER = <literal> ]
       WSDLURI = <literal>
       [ <endpoint> ]
       [ CHECKCERTIFICATES ]
       [ <pool configuration> ]
       [ <authentication> ]
       [ <proxy> ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]
   
   <endpoint> ::=
         ENDPOINT URI = <literal>
       | ENDPOINT VAR = <var_name:identifier>
   
   <pool configuration> ::= 
         MAXCONNECTIONS <integer>
         CONNECTIONPOOLTIMEOUT <integer>
         [ CONNECTIONPOOLREADTIMEOUT <integer> ]
   
   <authentication> ::= 
     AUTHENTICATION <authentication type>
   
   <authentication type> ::=
       OFF
     | { 
           HTTP BASIC 
         | HTTP DIGEST 
         | HTTP SPNEGO 
         | WSS BASIC 
         | WSS DIGEST 
       }
       ( {
             <credentials> 
           | WITH PASS-THROUGH SESSION CREDENTIALS ( <credentials> )
           | <credentials_with_vars> 
         }
       ) 
     | HTTP NTLM ( {
           <ntlm_credentials> 
         | WITH PASS-THROUGH SESSION CREDENTIALS ( <ntlm_credentials> )
         | <ntlm_credentials_with_vars> 
       } 
       ) 
   
   <credentials> ::= 
     USER <literal> PASSWORD <literal> [ ENCRYPTED ] 
   
   <credentials_with_vars> ::= 
     USER <literal> VAR <user:identifier> 
     PASSWORD <literal> VAR <password:identifier>
   
   <ntlm_credentials> ::= 
     <credentials> [ DOMAIN <literal> ]
   
   <ntlm_credentials_with_vars> ::= 
     <credentials_with_vars> [DOMAIN <literal> VAR <domain:identifier> ]
   
   <proxy> ::=
     PROXY { 
         OFF 
       | DEFAULT 
       | ON ( HOST <literal> PORT <integer> [ <credentials> ] ) 
       | AUTOMATIC ( PACURI <literal> )
     }

To modify a web service data source use ALTER DATASOURCE WS.

.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE WS statement
   :name: Syntax of the ALTER DATASOURCE WS statement

   ALTER DATASOURCE WS <name:identifier>
       WSDLURI = <literal>
       [ <endpoint> ]
       [ CHECKCERTIFICATES ]
       [ <pool configuration> ]
       [ <authentication> ]
       [ <proxy> ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]

..

   <endpoint> ::= (see :ref:`Syntax of the CREATE DATASOURCE WS statement`)

   <pool configuration> ::= (see :ref:`Syntax of the CREATE DATASOURCE WS statement`)
   
   <authentication> ::= (see :ref:`Syntax of the CREATE DATASOURCE WS statement`)

   <proxy>::= (see :ref:`Syntax of the CREATE DATASOURCE WS statement`)

|

Explanation of some of the parameters of these statements:

-  ``OR REPLACE``: If present and a data source with the same name exists,
   the current definition is substituted with the new one.

-  ``WSDLURI``: The URI to the WSDL file that defines the Web Service.
   The WSDL file defines one or several Web services, where each service
   may be comprised of different ports with one or several operations
   each. A data source for a Web service allows the creation of wrappers
   modeling any of the operations that it defines.
   
   The Server only retrieves the WSDL when you create wrappers over this
   data source and not when you query them.
   
   If you create a wrapper over a Web service data source whose
   ``WSDLURI`` does not exist, the Server marks the wrapper as incomplete
   and the queries to it will fail. Once you execute a query over this
   wrapper and the Server can retrieve the WSDL, the wrapper is marked as
   complete and the wrapper will be “queryable”.

-  Pool configuration. The Web service data sources use a pool of
   connections to retrieve the data. That is, each data source has its
   own pool of HTTP connections, in order to avoid creating a new one for
   each request and reuse the existing ones.
   
   When a user executes a query that involves a base view of this data
   source, the Server, instead of creating a connection for each request,
   it reuses a connection of the pool. The benefit of this is that if the
   connection already is established, the Server will obtain the response
   much faster.

   -  ``MAXCONNECTIONS``: Maximum number of connections that the pool of
      this data source will store.
      If the Server has to execute a request and there are no free
      connections in the pool, the pool creates a new one. If the pool
      reaches the ``MAXCONNECTIONS`` and the Server needs another
      connection, it will wait the number of milliseconds set by
      ``CONNECTIONPOOLTIMEOUT``. If this timeout is reached, the request
      will fail. If the value of this timeout is zero, the Server will wait
      indefinitely to obtain a new connection.
      If the data source uses "Pass-through session credentials", the
      Server creates a pool of connections for each user name of each data
      source.
   -  ``CONNECTIONPOOLREADTIMEOUT``: timeout of a connection, in
      milliseconds.
      Once the Server has obtained a connection from the pool, it sends an
      HTTP request to the target host. This property controls how much time
      the connection will wait for the source to begin returning data. If
      this timeout is reached, the query fails.

-  ``ENDPOINT``: If the WSDL points to an incorrect URL or does not contain
   one, we can make the new data source use another URL to connect to the
   source. If this parameter is not present, the new data source will use
   the URL of the WSDL. Otherwise, we can:

   -  Indicate an URL with the parameter ``ENDPOINT URI``.
   -  Indicate the name of a variable (parameter ``ENDPOINT VAR``). All the
      views created over this data source will have a field with the value
      of this parameter (``var_name``). This is useful when the end point
      changes regularly or is obtained from another source at runtime.

-  ``CHECKCERTIFICATES`` (optional): Adding this clause is equivalent to
   selecting the check box “Check certificates” in the configuration dialog
   of the data source, on the Administration Tool. The section :ref:`Importing
   SOAP Web Service Sources` of the Administration Guide explains when you
   should enable this option.

-  Pool configuration (optional): the parameters ``MAXCONNECTIONS`` and
   ``CONNECTIONPOOLTIMEOUT`` configure the values of the connection pool
   used by this data source. See more about this in the section :ref:`SOAP Web Service Sources` of the Administration Guide.

-  ``AUTHENTICATION``: the supported authentication methods are:

   -  **HTTP Basic** or **HTTP Digest**.
   -  **HTTP NTLM**. Uses the `Microsoft's NT LAN Manager (NTLM)
      Authentication Protocol <https://msdn.microsoft.com/en-us/library/cc236621.aspx>`_ to access Microsoft
      Windows servers. Virtual DataPort supports NTLM v1 and NTLM v2.
   -  **HTTP SPNEGO (Kerberos)**. Uses Kerberos tickets to sign the HTTP
      requests sent to the Web service. See the section “Connecting to a
      SOAP Web Service with Kerberos Authentication” for more information
      about this.
   -  **WSS Basic** and **WSS Digest**. WSS (`Web Services Security (WS-Security) <https://www.oasis-open.org/committees/wss/>`_) 
      is a standard for the implementation of security
      features in applications using Web services. Currently, Denodo
      supports the authentication profile “Username Token” 
      (`Web Services Security Username Token Profile 1.1. <http://docs.oasis-open.org/wss/v1.1/>`_).

When using the syntax of ``<credentials_with_vars>`` to create the data
source, the base views created over this data source will have two extra
fields (or three, if using the NTLM authentication) which value will be
used as credentials to access the Web service. The name of these fields
will be the value of the parameters ``VAR``.

For example, the views created over the following data source will have
two extra fields: ``login_var`` and ``password_var``, which value will
be used as credentials.

.. code-block:: vql

   CREATE DATASOURCE WS …
   …
   AUTHENTICATION HTTP BASIC (
     USER 'anonymous' VAR login_var
     PASSWORD 'anonymous' VAR password_var )
   …

If you use the modifier ``WITH PASS-THROUGH SESSION CREDENTIALS`` to
create the data source, when a user queries a view that uses this data
source, Virtual DataPort uses the credentials of this user to connect to
the Web service. With this modifier, the values of the parameters
``USERNAME`` and ``PASSWORD`` are used only by the Administration Tool
to connect to the database and show the operations of the Web service.
But not for querying tables or views of the database.

If you created a data source with ``WITH PASS-THROUGH SESSION CREDENTIALS``, but you want to
query a view of this data source with other credentials than the ones
used to connect to the Server, add the parameters ``USERNAME``
and ``PASSWORD`` to the ``CONTEXT``. These two parameters are only
taken into account when the data source has been created with the option
``WITH PASS-THROUGH SESSION CREDENTIALS``.

For example, if ``view1`` has been created with the option ``WITH PASS-THROUGH SESSION CREDENTIALS`` and you
execute this:

.. code-block:: vql

   SELECT * 
   FROM view1
   CONTEXT(
       USERNAME = 'admin'
     , PASSWORD = 'd4GvpKA5BiwoGUFrnH92DNq5TTNKWw58I86PVH2tQIs/q1RH9CkCoJj57NnQUlmvgvvVnBvlaH8NFSDM0x5fWCJiAvyia70oxiUWbToKkHl3ztgH1hZLcQiqkpXT/oYd' ENCRYPTED
     , DOMAIN = 'ACME_DOMAIN')

the Server will connect to the Web service with the username
``admin``, password ``password`` and domain
``acme_domain``, ignoring the credentials used by the user to
connect to the Server.

It is mandatory to add the token ``ENCRYPTED`` and enter the password encrypted. To encrypt the password, use the command ``ENCRYPT_PASSWORD``. For example:

.. code-block:: vql

   ENCRYPT_PASSWORD 'my_secret_password';


.. warning:: Users should be careful when enabling the cache for views
   that involve data sources with pass-through credentials enabled. The
   section “Considerations When Configuring Data Sources with Pass-Through
   Credentials” explains this in more detail.


-  ``PROXY`` (Optional). If the HTTP request is sent through a proxy, you
   have three options:

   -  ``DEFAULT``: the Server will use the default HTTP proxy configuration
      of the Server. See the section :ref:`Default Configuration of HTTP Proxy`
      of the Administration Guide to learn how to configure these default
      values.
   -  ``ON``: the Server will connect to the proxy server indicated with
      the parameters ``HOST`` and ``PORT``. If the proxy requires
      authentication, you also have to provide the credentials of the
      proxy.
   -  ``AUTOMATIC``: provide the URL of a ``proxy.pac`` file that contains
      the configuration parameters of the proxy.
  
-  ``TRANSFER_RATE_FACTOR``: relative measure of the speed of the network connection between the Denodo server and the data source. Use the default value (e.g. 1 for JDBC databases located on premises) if the data source is accessible through a conventional 100 Mbps LAN. Use higher values for faster networks and lower values for data sources accessible through a WAN.
   
   The cost optimizer uses this value when evaluating the cost of an execution plan. The default value is usually correct so you should not specify this parameter unless you have a deep knowledge of the cost optimizer. 
