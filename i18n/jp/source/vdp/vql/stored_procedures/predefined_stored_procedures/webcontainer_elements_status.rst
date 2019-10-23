=============================
WEBCONTAINER_ELEMENT_STATUS
=============================

.. rubric:: Description

The stored procedure ``WEBCONTAINER_ELEMENT_STATUS`` returns information about
the Web services (SOAP and REST) and widgets of all the Virtual DataPort
databases. You can filter by database, type of web service, name of the
service, look only for services that are deployed, etc. 

.. rubric:: Syntax

.. code-block:: bnf

   WEBCONTAINER_ELEMENT_STATUS (
         input_database_name : text 
       , <input_service>
       , <input_service_type>
       , input_service_name : text
       , input_description : text 
       , input_modified : boolean 
       , input_deployed : boolean
   )
   
   <input_service> ::=
         'webservice'
       | 'widget'
       
   <input_service_type> ::=
         'REST'
       | 'SOAP'
       | 'SOAP/REST'

*All the parameters are optional*. If a parameter is ``null``, the
procedure does not filter by that parameter.

-  ``input_database_name``: name of the database where you want to search for Web
   services and/or widgets. If ``null``, it searches all the databases for
   services.

-  ``input_service``: type of service you are looking for. The value is case
   insensitive.

   -  To search for widgets, enter ``"widget"``.
   -  To search for Web services, enter "``webservice"``.
   -  To search both, enter ``null``.

-  ``input_service_type``: type of service that you are looking for. Widgets do
   not have subtypes, so when you are looking for them, just enter the
   value ``widget`` in the ``service`` parameter and set this to ``null``.

   -  To search for REST Web services, enter ``"REST"``.
   -  To search for SOAP Web services, enter ``"SOAP"``.
   -  To search for Web services imported from Virtual DataPort 4.7 or
      earlier, enter ``"SOAP/REST"``.

-  ``input_service_name``: if provided, the procedure returns the services whose
   name *contains* this value. Take into account that if the parameter
   ``input_database_name`` is ``null``, the procedure returns the services of all
   the databases that match this name. Not just the one that you currently
   connected to.

-  ``input_description``: if provided, the procedure returns the services whose
   description *contains* this value.

-  ``input_modified``: if ``true``, it returns all the services that are deployed
   and that have been modified after being deployed. If ``false``, it
   returns all the services that are deployed and that have not been
   modified afterwards.

-  ``input_deployed``: if ``true``, it returns all the services that are
   deployed. If ``false``, it returns the services that are not deployed.

The procedure returns one row per web service, with these fields:

-  ``database_name``: database of the service (widget or web service).
-  ``service``: type of service. Possible values:

   -  ``webservice``
   -  ``widget``

-  ``service_type``: type of service. Possible values:

   -  ``REST``: for REST web services.
   -  ``SOAP``: for SOAP web services.
   -  ``null``: for widgets.
   
-  ``service_name``: name of the service (widget or web service).
-  ``ws_description``: description
-  ``deployed``: ``true`` if the service has been deployed in the web container of Denodo. ``false`` otherwise.
-  ``modified``: ``true`` if the service has been modified since it was deployed in the web container of Denodo. ``false`` otherwise.
-  ``ws_context``: context within the Denodo web container in which the service is deployed. ``null`` if the service has not been deployed in the web container of Denodo.
-  ``ws_user``: user account that the service uses to connect to the Server to execute queries. ``null`` if the service has not been deployed in the web container of Denodo.

.. rubric:: Privileges Required

This procedure only returns information about the Web services and
widgets that belong to databases on which the user is an administrator.
The implications of this are the following:

-  If the user is an administrator, the procedure will return
   information about all the Web services and widgets.
-  The procedure will return information about the Web services and
   widgets of the databases of which the user is a local administrator.

This procedure does not return a “privileges error”. For example, let us
say that:

-  A user requests information about the Web services of all the
   databases.
-  The Server has two databases: ``admin`` and ``testing``.
-  This user only is a local administrator of the ``testing`` database
   but not the ``admin`` database.

In this scenario, the procedure only returns information about the Web
services of the ``testing`` database and not about the Web services of
the ``admin`` database.

.. rubric:: Examples

.. rubric:: Example 1

*Obtain all the REST Web services of the database* ``test``:

.. code-block:: vql

   SELECT *
   FROM WEBCONTAINER_ELEMENT_STATUS()
   WHERE input_database_name = 'test'
       AND input_service_type = 'REST'

.. rubric:: Example 2

*Obtain all the deployed Web services and auxiliary Web services for
widgets*:



.. code-block:: vql

   SELECT *
   FROM WEBCONTAINER_ELEMENT_STATUS()
   WHERE input_deployed = true;

.. rubric:: Example 3

*Obtain all the deployed Web services whose definition has changed since
they were deployed*:


.. code-block:: vql
  
   SELECT *
   FROM WEBCONTAINER_ELEMENT_STATUS()
   WHERE input_deployed = true
       AND input_modified = true;
      
In this case, setting the last parameter (``deployed``) to ``true`` or
``null`` is equivalent because only deployed services are marked as
modified.

