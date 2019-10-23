============
Debug Option
============

For debug purposes, you can enrich the OData service responses 
with additional helpful data. To do this, follow these steps:

1. Edit the file :file:`{<DENODO_HOME>}/resources/apache-tomcat/webapps/denodo-odata4-service/WEB-INF/classes/configuration.properties`.
#. Set the property ``debug.enabled`` to ``true``
#. Restart the Virtual DataPort server in order for this change to take effect.
#. Add the query parameter ``odata-debug=json`` or ``odata-debug=html`` to the URL of the requests you want to debug. For example: 

   \http://denodo-server.acme.com:9090/denodo-odata4-service/denodo-odata.svc/movies?odata-debug=json.

The response will include the
parsed request URI, the server environment, library timings and the stack trace
in case an error occurred.

If in the configuration file you do not set the property to "true", the service will ignore the query parameter "odata-debug" and will not return debugging information.

If the URL does not have the query parameter "odata-debug", the service will not return debugging information regardless of the value of the property "debug.enabled".