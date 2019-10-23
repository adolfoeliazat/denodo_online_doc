=================
LDAP Data Sources
=================

Virtual DataPort allows using an LDAP server as a data source. Imported
LDAP servers can be used to extract data from them and also, to
authenticate Virtual DataPort users (see section :ref:`Managing Users`). The
following figures contain the syntax of the commands to manage LDAP data
sources:

To create an LDAP data source, use the statement CREATE DATASOURCE LDAP.

.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE LDAP statement
   :name: Syntax of the CREATE DATASOURCE LDAP statement

   CREATE [ OR REPLACE ] DATASOURCE LDAP <name:identifier>
       [ FOLDER = <literal> ]
       URI = <serverURI:literal>
       [ USERNAME = <userName:literal> USERPASSWORD = <password:literal> [ ENCRYPTED ] ]
       [ USE_GSSAPI_SASL_AUTH_MECHANISM ]
       [ USEPAGING = { TRUE | FALSE } [ MAXPAGESIZE = <integer> ] ]
       [ POOL_ENABLED = { TRUE | FALSE } ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]


To modify an LDAP data source, use the statement ALTER DATASOURCE LDAP.

.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE LDAP statement
   :name: Syntax of the ALTER DATASOURCE LDAP statement

   ALTER DATASOURCE LDAP <name:identifier>
       URI = <serverURI:literal>
       [ USERNAME = <userName:literal> ]
       [ USERPASSWORD = <password:literal> [ ENCRYPTED ] ]
       [ USE_GSSAPI_SASL_AUTH_MECHANISM ]
       [ USEPAGING = { TRUE | FALSE } [ MAXPAGESIZE = <integer> ] ]
       [ POOL_ENABLED = { TRUE | FALSE } ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]

|

Explanation of some of the parameters of these statements:  

-  ``OR REPLACE``. If present and a data source with the same name exists,
   the current definition is substituted with the new one.
-  ``name``. Name of the new data source in Virtual DataPort.
-  ``URI``. URI of the LDAP server. The URI format is
   ``ldap://host:port``.
-  ``USERNAME`` / ``USERPASSWORD``. Credentials to access the LDAP
   server (optional). The ``ENCRYPTED`` modifier indicates the password
   is provided encrypted. Usually, this modifier is only used by Virtual
   DataPort metadata import/export process (see section :ref:`Exporting
   Metadata`).
-  ``USE_GSSAPI_SASL_AUTH_MECHANISM``. If present, a SASL binding (with GSSAPI authentication mechanism) will be established instead of a simple binding.
-  ``USEPAGING``. If ``TRUE``, Virtual DataPort will do paged searches
   to obtain all the results of the queries, instead of obtaining all
   the results at once. ``MAXPAGESIZE`` is the number of results per
   page.
   
   This option is useful if the LDAP server has a limit on the number of results per query.
   
-  ``POOL_ENABLED``. If ``TRUE``, Virtual DataPort create a pool of
   connections to this LDAP server.

-  ``TRANSFER_RATE_FACTOR``. Relative measure of the speed of the network connection between the Denodo server and the data source. Use the default value (e.g. 1 for JDBC databases located on premises) if the data source is accessible through a conventional 100 Mbps LAN. Use higher values for faster networks and lower values for data sources accessible through a WAN.

|

See more details about this in the section :ref:`LDAP Sources` of the Administration Guide.

