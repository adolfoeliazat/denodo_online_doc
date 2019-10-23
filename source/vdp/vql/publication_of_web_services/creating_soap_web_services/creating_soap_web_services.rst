==============================
Creating SOAP Web Services
==============================

Use the statement CREATE SOAP WEBSERVICE to create a SOAP web service that publish views and/or
stored procedures.

.. code-block:: bnf
   :caption: Syntax of the CREATE SOAP WEBSERVICE statement
   :name: Syntax of the CREATE SOAP WEBSERVICE statement

   CREATE [ OR REPLACE ] SOAP WEBSERVICE <name:identifier>
       CONNECTION (
           CHUNKSIZE = <integer>
           CHUNKTIMEOUT = <integer>
           QUERYTIMEOUT = <integer>
           POOLENABLED = <boolean>
           POOLINITSIZE = <integer>
           POOLMAXACTIVE = <integer>
       )
       I18N <name:identifier>
       [ DATETYPEMAPPING { DATE | DATETIME } ]
       [ NOWRAPARRAYS ]
       [ RAISEFAULTONIDU ]
       [ DONOTAPPLYOUTPUTXSLTTOERRORS ]
       [ VERBOSEERRORS = <boolean> ] // Default value is true
       [ AUTHENTICATION ( <authentication> ) ]
       OUTPUT (
           STYLE { DOCUMENT | RPC }
           [ XSLT ( <XSLT configuration> [, <XSLT configuration> ]* ) ]
           [ JMS (
               VENDOR { ACTIVEMQ | WEBSPHEREMQ | JNDI }
               DESTINATION = <name:literal>
               [ REPLYTO = <name:literal> ]
               { QUEUE | TOPIC }
               [ USER = <name:literal> PASSWORD = <name:literal> [ ENCRYPTED ] ]
               PROPERTIES ( <properties> [, <properties> ]* )
            )]
       )
       [ <operation> ]+
       [ FOLDER = <literal> ]
       [ DESCRIPTION = <literal> ]

   CREATE [ OR REPLACE ] SOAP WEBSERVICE <name:identifier>
         CONNECTION (
             CHUNKSIZE = <integer>
             CHUNKTIMEOUT = <integer>
             QUERYTIMEOUT = <integer>
             POOLENABLED = <boolean>
             POOLINITSIZE = <integer>
             POOLMAXACTIVE = <integer>
         )
         I18N <name:identifier>
         [ DATETYPEMAPPING { DATE | DATETIME } ]
         [ NOWRAPARRAYS ]
         [ RAISEFAULTONIDU ]
         [ DONOTAPPLYOUTPUTXSLTTOERRORS ]
         [ VERBOSEERRORS = <boolean> ] // Default value is true
         [ AUTHENTICATION ( <authentication> ) ]
         OUTPUT (
             STYLE { DOCUMENT | RPC }
             [ XSLT ( <XSLT configuration> [, <XSLT configuration> ]* ) ]
             [ JMS (
                 VENDOR { ACTIVEMQ | WEBSPHEREMQ | JNDI }
                 DESTINATION = <name:literal>
                 [ REPLYTO = <name:literal> ]
                 { QUEUE | TOPIC }
                 [ USER = <name:literal> PASSWORD = <name:literal> [ ENCRYPTED ] ]
                 PROPERTIES ( <properties> [, <properties> ]* )
              )]
         )
         [ <operation> ]+
         [ FOLDER = <literal> ]
         [ DESCRIPTION = <literal> ]

   <authentication> ::=
       BASIC <credentials>
     | BASIC VDP [ VDPACCEPTEDUSERS <users_list> ]
     | DIGEST <credentials>
     | SPNEGO
     | WSS BASIC <credentials>
     | WSS BASIC LDAP <ldap_credentials>
     | WSS BASIC VDP [ VDPACCEPTEDUSERS <users_list> ]
     | WSS DIGEST <credentials>

   <credentials> ::=
     USER <user_name:literal> PASSWORD <password:literal> [ ENCRYPTED ]

   <users_list> ::=
     <user_name:literal> [, <user_name:literal> ]*

   <operation> ::=
     OPERATION <name:literal> (
       TYPE { SELECT | INSERT | UPDATE | DELETE }
       SCHEMA { VIEW | WRAPPER <wrapper type> | STOREDPROCEDURE }
         <schema name:literal>
       VQL = <literal>
       INPUTS [ <input element name:literal> ] ( [ <input parameter> ]* )
       OUTPUT <return parameter>
     )

   <input parameter> ::=
     [ <input parameter type> ] <name:literal> <name in the view/query:literal>
     [ : <type:literal> ] <operator:literal> [ OBL ]
     [ ( <renamed field> ) ]

   <input parameter type> ::=
       ORDERBY
     | OFFSET
     | FETCH

   <return parameter> ::=
       <simple type:literal>
     | <register type:literal> : ARRAY OF ( <return parameter register> )
       [ <renamed field> [, <renamed field> ]* ]

   <return parameter register> ::=
     <name:literal> : REGISTER OF ( <register field> [, <register field> ]* )

   <register field> ::=
     <name:literal> [ : <type:literal> ]

   <renamed field> ::=
     <XPath of the field:literal> = <displayed Name:literal>
     [ : [ <displayed array type name:literal> / ]
     <displayed type name: literal> ]

   <XSLT configuration> ::=
     OPERATION = <name:literal>
     [ SOAPACTION = <action:literal> ]
     [ INPUTXSLT = <xslt:literal> <isEnabled> ]
     [ OUTPUTXSLT = <xslt:literal <isEnabled> ]

   <isEnabled> ::=
       ENABLED
     | DISABLED

   <properties> ::=
     <key:literal> = <value:literal>

   <wrapper type> ::=
       ARN | CUSTOM | DF | ESSBASE | GS | ITP | JDBC | JSON
     | LDAP | ODBC | OLAP | SALESFORCE | SAPBWBAPI | SAPERP | WS | XML }


A SOAP Web service published by Virtual DataPort is formed by a list of
operations defined using the ``OPERATION`` clause. Each operation will
run a VQL statement that is indicated in the ``VQL`` property of the
operation. The operation may act on a view, a wrapper, a stored
procedure or execute a specific VQL statement (``SCHEMA`` property). The
type of the statement (``TYPE`` property) can be: ``SELECT`` (most
common), ``INSERT``, ``UPDATE`` or ``DELETE``. Each operation contains a
list of input parameters and one output parameter. The output parameter
of query operation will be an array of registers containing the results
of the query run. Insert / update / delete operations return the number
of tuples affected by the operation.

The parameters ``CHUNKSIZE``, ``CHUNKTIMEOUT``, ``QUERYTIMEOUT``,
``POOLENABLED``, ``POOLINITSIZE`` and ``POOLMAXACTIVE`` configure the
connection that Web service establishes with the Virtual DataPort server
(see section :doc:`/vdp/vql/publication_of_web_services/connection_parameters/connection_parameters`).

The ``I18N`` parameter indicates the internationalization configuration
used by the service.

The ``DATETYPEMAPPING`` parameter configures how the values of type
``date`` will be formatted:

-  If it is ``DATE``, the Server will only print the day, month and
   year.
-  If it is ``DATETIME``, it includes the time as well.

The authentication settings of the Service are controlled with the
``AUTHENTICATION`` parameter.

The clause ``NOWRAPARRAYS`` modifies the SOAP output of the Web service
only if the value of the clause ``STYLE`` is ``DOCUMENT``. If this
clause is present, the output of the SOAP Web service is simpler than if
it is not. However, the output is not backward compatible with the SOAP
Web services published from previous versions of the Denodo Platform. We
recommend adding it. Adding this clause is equivalent to clearing the
“Old SOAP DOCUMENT output” check box of the “Publish SOAP Web service”
wizard of the Administration Tool.

If the clause ``RAISEFAULTONIDU`` is present, the Web service raises a
SOAP fault when an insert, delete or update operation fails. Adding this
clause is equivalent to clearing the “Ignore faults on IDU operations”
check box of the “Publish SOAP Web service” wizard of the Administration
Tool.

The SOAP Web services can subscribe to a JMS server to listen to SOAP
messages (`SOAP over JMS <https://www.w3.org/TR/soapjms/>`_). To do that, add the
parameter ``JMS`` and its appropriate parameters. The section :ref:`Defining
JMS Listeners` explains the meaning of the parameters related to
establishing connections with JMS servers.

In an environment with existing SOAP clients and services, you do not
need to modify those clients to work with Virtual DataPort Web services.
You can define XSLT stylesheets to transform the incoming SOAP messages
to adapt them to the format that the new Web service expects. We also
can define stylesheets to transform the SOAP responses before sending
them to the existing clients. To do this, use the parameter ``XSLT``.

By default, the XSLT transformations are applied to data but not to the
error messages returned by the Web service. If the clause
``DONOTAPPLYOUTPUTXSLTTOERRORS`` is present, the Service applies these
transformations to error messages as well.

If you do not want the Web service to return verbose error messages, add
the clause ``VERBOSEERRORS = false``. If you do not add this clause or
set it to ``true``, the Web service will return verbose error messages
when there is an error invoking one of its operations. These messages
indicate where the problem was raised. E.g. if there was a timeout
connecting to one of the data sources, an error executing a query, etc.
If you do not want the clients of the Web service to get these verbose
messages, add ``VERBOSEERRORS = false``.

For more information about this, read the section :ref:`XSLT
Transformations` of the Administration Guide.

``PRESERVE_OPERATIONS`` clause can be used only with a ``CREATE OR REPLACE``
operation. The token indicates that if exists a web service with the same name,
its operations will be preserved and replaced when operation name conflict.
