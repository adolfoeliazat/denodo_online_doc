==================================================================
Web Services Created with Previous Versions of the Denodo Platform
==================================================================

In the previous versions of the Denodo Platform, there was only one type
of Web service, which combined the SOAP and REST representations. In the
current version of the Platform, the Web services have been divided into
two: the SOAP ones and the REST ones because the REST Web services have
changed from an “operation-oriented” approach to a “RESTful” approach.

The Web services created with the previous versions of the Platform
still work, but they are deprecated. This appendix explains how to
manage the old Web services.

The :ref:`Syntax of the CREATE SOAP WEBSERVICE statement` shows the
syntax for creating a new Web service that publishes a view or a stored
procedure.



.. code-block:: bnf
   :caption: Syntax of the CREATE WEBSERVICE statement
   :name: Syntax of the CREATE WEBSERVICE statement

   CREATE [ OR REPLACE ] WEBSERVICE <name:identifier>
       CHUNKSIZE = <integer>
       CHUNKTIMEOUT = <integer>
       QUERYTIMEOUT = <integer>
       POOLENABLED = <boolean>
       POOLINITSIZE = <integer>
       POOLMAXACTIVE = <integer>
       I18N <name:identifier>
       [ DATETYPEMAPPING { DATE | DATETIME } ]
       [ NOWRAPARRAYS ]     [ AUTHENTICATION ( HTTP <http_authentication> SOAP <soap_authentication> )
       OUTPUT TYPE ( [ 
            REST [ ( OPCONFIGS( <rest_xml_config> [, <rest_xml_config>]* ) )]      | JSON [ ( OPCONFIGS( <rest_config> [, <rest_config>]* ) )]
          | SOAP ( STYLE { DOCUMENT | RPC } 
              [ XSLT ( <xslt_config> [, <xslt_config> ]* ) ]
              [ JMS ( 
                  VENDOR { ACTIVEMQ | WEBSPHEREMQ | JNDI }
                  DESTINATION = <name:literal>
              [ REPLYTO = <name:literal> ]
              { QUEUE | TOPIC }
              [ USER = <name:literal> 
                PASSWORD = <name:literal> [ ENCRYPTED ] 
              ]
              PROPERTIES ( <properties> [, <properties> ]* )
                )
              ]       )
          | HTML [ ( CSS = <css:literal> ) ]
          | RSS ( [ <rss mapping> ]* 
                  [OPCONFIGS( <rest_config> [, <rest_config> ]* ] ) ) 
       ]+ )
       [ <operation> ]+ 
       [ FOLDER = <literal> ]
       [ DESCRIPTION = <literal> ]
       [ CASE_SENSITIVE = <boolean> ]
   
   <http_authentication> ::= {
         NONE 
       | BASIC <credentials> 
       | BASIC VDP [ VDPACCEPTEDUSERS <users_list> ]
       | DIGEST <credentials> 
   }
   
   <soap_authentication> ::= {
         NONE
       | BASIC <credentials>  
       | BASIC VDP [ VDPACCEPTEDUSERS <users_list:users_list> ]
       | DIGEST <credentials> 
       | WSS BASIC <credentials> 
       | WSS BASIC VDP [ VDPACCEPTEDUSERS <users_list:users_list> ]
       | WSS DIGEST <credentials> 
   }
   
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
       INPUTS ( [ <inputparameter> ]* )
       OUTPUT <returnparameter>
     )
   
   <inputparameter> ::=
      <paramName:literal> <name in the view/query:literal> 
      [ : <paramType:literal> ] <operator:literal> [ OBL ]
      [ ( <renamed field> ) ] 
                  
   <returnparameter> ::=
       <simpleType:literal>
     | <regType:literal> : ARRAY OF ( <returnparameter_register> ) [ <renamed field> [, <renamed field> ]* ]
   
   <returnparameter_register> ::= 
     <regName:literal> : REGISTER OF ( <registerfield> [, <registerfield>]* )
   
   <registerfield> ::=
     <fieldName:literal> [: <fieldType:literal>]
   
   <renamed field> ::= 
     <XPath of the field:literal> = <displayed Name:literal>
                      
   <rss mapping> ::=
     MAPPING { VIEW | STOREDPROCEDURE | WRAPPER } 
       <schemaName:literal> (  
         CHANNEL ( <mapping> [, <mapping> ]* ) 
         ITEM ( <itemMapping> [, <itemMapping> ]* )
       ) 
                   
   <xslt_config> ::=
     OPERATION = <name:literal>
     [ SOAPACTION = <action:literal> ]
     [ INPUTXSLT = <xslt:literal> <isEnabled> ]
     [ OUTPUTXSLT = <xslt:literal <isEnabled> ]
   
   <isEnabled> ::= 
     ENABLED | DISABLED  <rest_config> ::= 
       OPERATION = <name:literal>
       CUSTOMURL = url:literal
   
   <rest_xml_config> ::=
       OPERATION = <name:literal>
       [ CUSTOMURL = url:literal 
         CUSTOMNAMESPACE = ns:literal
         <rest_xslt_config>
         PRETTYOUTPUT
       ]+

   <rest_xslt_config> ::=
       [ INPUTXSLT = <xslt:literal> <isEnabled> ]
       [ OUTPUTXSLT = <xslt:literal <isEnabled> ]
   
   <properties> ::=
       <key:literal> = <value:literal>
   
   <mapping> ::=
       <rss tag:literal> = <value:literal>
   
   <item mapping> ::=
       <rss tag:literal> = <field:identifier>
       
   <wrapper type> ::=
       ARN 
     | CUSTOM
     | DF 
     | ESSBASE 
     | GS 
     | ITP 
     | JDBC 
     | JSON
     | LDAP 
     | ODBC 
     | SALESFORCE
     | SAPBWBAPI 
     | SAPERP 
     | WS 
     | XML

Below is a brief description of the use of the statement. A Web service
published by Virtual DataPort is formed by a list of operations defined
using the ``OPERATION`` clause. Each operation will run a VQL statement
that is indicated in the ``VQL`` property of the operation. The
operation may act on a view, a wrapper, a stored procedure or execute a
specific VQL statement (``SCHEMA`` property). The type of the statement
(``TYPE`` property) can be: select (most common), insert, update or
delete. Each operation contains a list of input parameters and one
output parameter. In the case of query operations, the output parameter
will be an array of registers containing the results of the query run.
Insert / update and delete operations return the number of tuples
affected by the operation.

The versions in which the service is to be published are indicated in
the ``OUTPUT_TYPE`` clause. The ``I18N`` parameter allows specifying the
internationalization configuration used by the service.

The parameters ``CHUNKSIZE``, ``CHUNKTIMEOUT``, ``QUERYTIMEOUT``,
``POOLENABLED``, ``POOLINITSIZE`` and ``POOLMAXACTIVE`` control the
connection between the Web service and the Virtual DataPort server.

The section :doc:`/vdp/vql/publication_of_web_services/connection_parameters/connection_parameters` explains the meaning of these
parameters.

The clause ``NOWRAPARRAYS`` modifies the SOAP output of the Web service
only if the value of the clause ``STYLE`` is ``DOCUMENT``. If this
clause is present, the output of the SOAP Web service is simpler than if
it is not. However, the output is not backward compatible with the SOAP
Web services published from previous versions of the Denodo Platform. We
recommend adding it.

Having this clause in your ``CREATE WEBSERVICE`` statement is equivalent
to clearing the **Old SOAP DOCUMENT output** check box of the Publish
Web service wizard.

The RSS format specifies a series of specific fields for each item.
Therefore, on exporting a view in RSS format, the correspondence between
the fields of the view and the fields in RSS format must be specified.
This is possible through the ``MAPPING`` clause. An RSS feed contains
a ``channel`` element that specifies general information on the feed.
The ``CHANNEL`` parameter of the ``MAPPING`` clause allows specifying
constant values for each of the ``channel`` subelements permitted by the
RSS format. An RSS feed contains a list of ``item`` elements. Virtual DataPort
will generate an ``item`` element for each tuple returned by the query
made on the view or stored procedure used in the service. The ``ITEM``
parameter of the ``MAPPING`` clause allows selecting the attribute of
the view corresponding to each ``item`` subelement defined in RSS
format. The RSS format specifies that at least one value must be
assigned either to the ``title`` subelement or to the ``description``
subelement.



The Web services published by Virtual DataPort can subscribe to a JMS
server to listen to SOAP messages (`SOAP over Java Message Service <https://www.w3.org/TR/soapjms/>`_).
To do that, add the parameter ``JMS`` and its appropriate parameters.
The section :ref:`Defining JMS Listeners` explains the meaning of the
parameters related to establishing connections with JMS servers.

In an environment with existing SOAP/REST clients and services, we do
not need to modify those clients to work with Virtual DataPort Web
services. We can define XSLT stylesheets (`XSL Transformations <https://www.w3.org/TR/xslt>`_) to
transform the incoming SOAP or REST messages to adapt them to the format
that the new Web service expects. We also can define stylesheets to
transform the SOAP and REST responses before sending them to the
existing clients. To do this, use the parameter ``XSLT``.

For more information about this, read the section :doc:`/vdp/administration/publication_of_web_services/xslt_transformations/xslt_transformations`
of the Administration Guide.

Add the clause ``CASE_SENSITIVE = true`` if you want the input fields of
the REST version of the Web Service to be case sensitive. If ``false``
(default value), the input parameters are case insensitive.

E.g. if the ``CASE_SENSITIVE = false`` and the service has a parameter
called REGION, in the URL you can provide the parameter “REGION” or
“ReGiOn” and the service will return a response.
