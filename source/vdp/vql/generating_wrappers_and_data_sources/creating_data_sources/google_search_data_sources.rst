==========================
Google Search Data Sources
==========================

The statement CREATE DATASOURCE GS creates a data source that points to a
`Google Search <https://enterprise.google.com/search//>`_ service.


.. code-block:: bnf
   :caption: Syntax of the CREATE DATASOURCE GS statement (Google Search)
   :name: Syntax of the CREATE DATASOURCE GS statement (Google Search)

   CREATE [ OR REPLACE ] DATASOURCE GS <name:identifier>
       [ FOLDER = <literal> ]
       GSURI = <literal>
       [ PROXY {
             OFF
           | DEFAULT
           | ON ( HOST <literal> PORT <integer>
                 [ USER <literal> PASSWORD <literal> [ ENCRYPTED ] ] )
               | AUTOMATIC ( PACURI <literal> )
           }
       ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]

To modify a Google Search data source, use the statement ALTER DATASOURCE GS.

.. code-block:: bnf
   :caption: Syntax of the ALTER DATASOURCE GS statement (Google Search)
   :name: Syntax of the ALTER DATASOURCE GS statement (Google Search)

   ALTER DATASOURCE GS <name:identifier>
       GSURI = <literal>
       [ PROXY {
             OFF
           | DEFAULT
           | ON ( HOST <literal> PORT <integer>
               [ USER <literal> PASSWORD <literal> [ ENCRYPTED ] ] )
           | AUTOMATIC ( PACURI <literal> )
           }
       ]
       [ TRANSFER_RATE_FACTOR = <double> ]
       [ DESCRIPTION = <literal> ]


|

Explanation of some of the parameters of these statements:  

-  ``name``. Name to be given to the data source in Virtual DataPort.

-  ``GSURI``. Access URI to the Google Search server. The URI format is
   ``host:port``, being ``host`` the name of the machine that hosts the
   search engine.

-  ``PROXY``. If the HTTP request is sent through a proxy, you have three
   options:

   -  ``DEFAULT``: the data source will use the default HTTP proxy
      configuration of the Server. See the section :ref:`Default Configuration
      of HTTP Proxy` of the Administration Guide to learn how to
      configure these default values.
   -  ``ON``: the data source will connect to the proxy specified by the
      parameters ``HOST`` and ``PORT``. If the proxy requires
      authentication, you also have to provide the credentials of the
      proxy.
   -  ``AUTOMATIC``: provide the URL of a ``proxy.pac`` file that contains
      the configuration parameters of the proxy.
  

-  ``TRANSFER_RATE_FACTOR``: relative measure of the speed of the network connection between the Denodo server and the data source. Use the default value (e.g. 1 for JDBC databases located on premises) if the data source is accessible through a conventional 100 Mbps LAN. Use higher values for faster networks and lower values for data sources accessible through a WAN.
