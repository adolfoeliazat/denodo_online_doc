=============
Configuration
=============

The configuration of the OData service is in the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/denodo-odata4-service/WEB-INF/classes/configuration.properties`.

In this file you can modify the following properties:

* ``odataserver.address``: service name. For example, if you change this to ``/odata-interface.svc``, the URL to access the service will be ``http://localhost:9090/denodo-odata4-service/odata-interface.svc/<database name>/``.
* ``odataserver.serviceRoot`` (optional): root URI of the service. Set the value of this property if the service
  is going to be accessed through a gateway, so links within the OData
  response use this URI as root. For example, if the value of this property is ``https://gw.denodo.com:9000/ODATA/``, 
  a request to 
  ``http://server001:9090/denodo-odata4-service/denodo-odata.svc/movies``
  will return this URL in the response: 
  ``https://gw.denodo.com:9000/ODATA/denodo-odata.svc/movies``.
* ``server.pageSize``: number of records returned per request. See more about this in the section 
  :ref:`Pagination section <vdp_admin_guide_odata4_service_pagination>`.        
* ``enable.adminUser``: if ``false`` the user account ``admin`` (not other administrator accounts) will not be able to access the OData service. This functionality can be used only with Basic 
  authentication, not with Kerberos authentication.
* ``disable.kerberosAuthentication``: set to ``true`` to disable Kerberos authentication in the connections opened by this service to the Virtual DataPort server. If ``true``, configure the service to allow Basic authentication.
* ``disable.basicAuthentication``: set to ``true`` to forbid HTTP Basic authentication in the requests to the OData service. If ``true``, Kerberos authentication has to be enabled.
* ``debug.enabled``: if ``true``, the responses to requests with the query parameter ``?odata-debug=json`` will include debugging information, in addition to the data itself.

The default values for these properties are the following:

.. code-block:: properties

   odataserver.address=/denodo-odata.svc
   odataserver.serviceRoot=
   server.pageSize=1000
   enable.adminUser=true
   disable.kerberosAuthentication=false
   disable.basicAuthentication=false
   debug.enabled=false 

In order for changes to these properties to take effect, restart the Virtual DataPort server.

Kerberos Authentication
=======================

The OData Service supports Single Sign-On (using the Kerberos
protocol). Follow these steps to enable this:

#. Perform the post-installation tasks described in the section :ref:`Setting-up
   Kerberos Authentication` of the Installation Guide.

#. Enable Kerberos in the Virtual DataPort Server, as
   described in the section :ref:`Setting-Up the Kerberos Authentication in the
   Virtual DataPort Server` of the Administration Guide.

#. In the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/denodo-odata4-service/WEB-INF/classes/configuration.properties`, set the property ``disable.kerberosAuthentication`` to ``false``.

#. `Configure Kerberos in your browser
   <https://www.oracle.com/technetwork/articles/idm/weblogic-sso-kerberos-1619890.html>`_.

.. important:: To access the Denodo OData 4.0 Service, use the Fully Qualified Domain Name of the Server Principal Name you
   configured in the Virtual DataPort Server. For
   example, if your Server Principal Name is
   ``HTTP/denodo-prod.subnet1.contoso.com@CONTOSO.COM``, you should access the
   Denodo OData 4.0 Service through the URL
   ``http://denodo-prod.subnet1.contoso.com:9090/denodo-odata4-service/denodo-odata.svc`` or 
   ``https://denodo-prod.subnet1.contoso.com:9443/denodo-odata4-service/denodo-odata.svc``.
   
Enabling Cross-Origin Resource Sharing (CORS)
=============================================

This service supports Cross-origin resource
sharing (`CORS <https://www.w3.org/TR/cors/>`_). 
The section :ref:`vdp-admin-publish-ws-settings-tab-cors` (settings of published REST web services) explains what CORS is.

To enable the CORS support on the OData Service, follow these steps:

#. Edit the file ``web.xml`` of the folder :file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/denodo-odata4-service/WEB-INF/`.
#. Add the following *at the top* of this file.

   .. code-block:: none

      <!DOCTYPE doc [
      <!ENTITY cors_settings SYSTEM "cors_settings.xml">
      ]>

   The file has to end up looking like:
   
   .. code-block:: none
      :emphasize-lines: 4

      <!DOCTYPE doc [
      <!ENTITY cors_settings SYSTEM "cors_settings.xml">
      ]>
      <web-app ...
	
   And add the reference in the filters section:

   .. code-block:: xml

      <!-- ******************* -->
      <!-- Filters             -->
      <!-- ******************* -->
      &cors_settings;

#. Edit the file ``cors_settings.xml`` of the folder :file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/denodo-odata4-service/WEB-INF/`.
#. Uncomment the contents of the file.
#. By default, the value of the property ``cors.allowed.origins`` is \*. With this, the service will allow CORS
   requests received from any domain.
   
   To limit the domains from which CORS requests are allowed, change 
   the value of this property: replace \* with the list of allowed URLs (separate each URL by a comma).

   For example, ``https://foo.com, http://foo.com, https://foo.bar.com`` 

   CORS requests from any other origin will be denied with the HTTP code 403 (Forbidden)

   .. important:: For each URL, enter its protocol as well. URLs that not contain 
      the protocol are invalid. E.g. **foo.com** is invalid.
   
#. Restart the Virtual DataPort server to apply the changes.
