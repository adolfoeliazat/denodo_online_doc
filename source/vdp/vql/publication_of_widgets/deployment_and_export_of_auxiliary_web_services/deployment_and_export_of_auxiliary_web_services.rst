===============================================
Deployment and Export of Auxiliary Web Services
===============================================

.. note:: Publishing views as widgets is a deprecated feature and it may be removed in future
   major versions of the Denodo Platform.
   
   The section :ref:`Features Deprecated in Virtual DataPort 7.0` lists all the features that are deprecated.

The auxiliary web service is a special web service used by Microsoft Web
Parts to obtain the data from the views or stored procedures of the
Virtual DataPort server.

This Web Service can be deployed into the web container embedded in the
Denodo Platform; or it can also be exported to a *.war* file and
deployed into another J2EE Application Server.

Remember that these web services have to be deployed before using the
widgets that need them.

The following figures show the syntax of the commands available to
manage these web services.



.. code-block:: vql
   :name: Syntax of the DEPLOY WIDGET WEBSERVICE, REDEPLOY WIDGET WEBSERVICE, UNDEPLOY WIDGET WEBSERVICE and EXPORT WIDGET WEBSERVICE statements
   :caption: Syntax of the DEPLOY WIDGET WEBSERVICE, REDEPLOY WIDGET WEBSERVICE, UNDEPLOY WIDGET WEBSERVICE and EXPORT WIDGET WEBSERVICE statements

   DEPLOY WIDGET WEBSERVICE <name:identifier>
   [
       LOGIN = <literal> 
       PASSWORD = <literal> [ ENCRYPTED ]
   ]
   
   REDEPLOY WIDGET WEBSERVICE <name:identifier>
   [
       LOGIN = <literal> 
       PASSWORD = <literal> [ ENCRYPTED ]
   ]
   
   UNDEPLOY [IF EXISTS] WIDGET WEBSERVICE <name:identifier>
   
   EXPORT WIDGET WEBSERVICE <widget name:identifier> 
       NAME = <output name:literal>
       URI = <literal>  
       LOGIN = <literal> 
       PASSWORD = <literal> [ ENCRYPTED ]
   
In this command the parameter ``URI`` is the URL of the Virtual DataPort
server that the web service will retrieve the data from.

The parameters ``LOGIN`` and ``PASSWORD`` are the credentials that the
Web service of the widget will use to open a connection to the Virtual
DataPort server and execute queries.

In the statements ``DEPLOY WIDGET WEBSERVICE`` and
``REDEPLOY WIDGET WEBSERVICE``, these parameters are optional. If they
are not present, the Web service is configured with the credentials of
the user that executes the ``DEPLOY`` or the ``REDEPLOY``.

The credentials used by the Web service are stored in its own
configuration file. So, if the user used by the Web service is deleted or
its password changes, you have to redeploy the Web service.

| 

The following command displays a list of the auxiliary web services
deployed in the embedded container:



.. code-block:: vql
   :caption: Parameter to obtain the status of the embedded Web container
   :name: Parameter to obtain the status of the embedded Web container

   WEBCONTAINER WIDGETS STATUS
