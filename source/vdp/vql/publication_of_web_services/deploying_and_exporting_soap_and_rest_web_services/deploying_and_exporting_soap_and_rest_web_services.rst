==================================================
Deploying and Exporting SOAP and REST Web Services
==================================================

The statements to deploy, redeploy and undeploy a Web service are the
same for the REST and the SOAP Web services: ``DEPLOY WEBSERVICE``,
``REDEPLOY WEBSERVICE`` and ``UNDEPLOY WEBSERVICE``.

If you want to deploy the Service to an external J2EE Web container,
execute the ``EXPORT WAR FROM WEBSERVICE`` statement to generate a
``war`` file containing the Web service specified in the ``WEBSERVICE``
parameter. The file name will be as specified in the ``NAME`` parameter.

To generate the WSDL file of a SOAP Web service, execute the
``EXPORT WSDL FROM WEBSERVICE`` statement. The output file name will be
as specified in the ``NAME`` parameter.

Both the ``war`` and the ``wsdl`` files will be accessible in the
:file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/export` directory or in
the URL ``http://localhost:9090/export``.

.. code-block:: bnf
   :caption: Syntax of the DEPLOY, REDEPLOY, UNDEPLOY, EXPORT WAR and EXPORT WSDL statement
   :name: Syntax of the DEPLOY, REDEPLOY, UNDEPLOY, EXPORT WAR and EXPORT WSDL statement

   DEPLOY WEBSERVICE <name:identifier> 
   [
       LOGIN = <literal> 
       PASSWORD = <literal> [ ENCRYPTED ]
   ]

   REDEPLOY WEBSERVICE <name:identifier> 
   [
       LOGIN = <literal> 
       PASSWORD = <literal> [ ENCRYPTED ] ]
   
   UNDEPLOY [IF EXISTS] WEBSERVICE <name:identifier>
   
   EXPORT WAR FROM WEBSERVICE <name:identifier> 
       NAME = <name:literal> 
       URI = <literal>  
       LOGIN = <literal> 
       PASSWORD = <literal> [ ENCRYPTED ]
       [ SAMLBASEURL = <SAML 2.0 base URL:literal> ]
   
   EXPORT WSDL FROM WEBSERVICE <name:identifier> 
       NAME = <name:literal>
   
The parameters ``LOGIN`` and ``PASSWORD`` are the credentials that the

Web service will use to open a connection to the Virtual DataPort server
and execute queries.

In the statement ``DEPLOY WEBSERVICE``, if the parameters ``LOGIN`` and
``PASSWORD`` are not present, the Web service will use the credentials
of the user that executes this statement.

In the statement ``REDEPLOY WEBSERVICE``, if the parameters ``LOGIN``
and ``PASSWORD`` are not present, the Web service will use the same
credentials that it was originally deployed with. For example, if the
service was deployed by the user “scott” and then, the user “bruce”
executes ``REDEPLOY WEBSERVICE`` without adding the ``LOGIN`` and
``PASSWORD`` parameters, the service will still use the credentials of
the user “scott”.

The credentials used by the Web service are stored in its own
configuration file. So, if the user used by the Web service is deleted or
its password changes, you have to redeploy the Web service.

