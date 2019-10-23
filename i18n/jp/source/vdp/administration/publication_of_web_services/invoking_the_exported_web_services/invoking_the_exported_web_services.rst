==================================
Invoking Denodo Web Services
==================================

The **Context Path** column of the :ref:`Web services container table<Web Service Container Status Table>` displays the URL of the services that are deployed in the embedded web container.

|

The starting URL for SOAP web services is

``http://acme:9090/server/<database name>/<web service name>/services``

This page contains a link to the WSDL of the service.

|

The starting URL for REST web services is

``http://acme:9090/server/<database name>/<web service name>/``

Access this URL to see the views exported by this service.

REST web services are invoked in the same way as the :ref:`Denodo RESTful web
service<RESTful Web service>` and provides support for
processing IDU requests (Insert, Delete and Update) and four
representations of data: HTML, XML, JSON and RSS.

Invoking Web Services with SAML Authentication
=============================================================

You can publish web services that require SAML 2.0 authentication (Security Assertion Markup Language). These services implement the profile “Web Browser SSO Profile” identity provider initiated with “HTTP POST Binding”.

To invoke these services, client applications need to obtain a SAML assertion and then, send the request to the REST service with this assertion. They need one assertion for each request because these REST services are stateless so they do not support sessions. 

After deploying the web service, its SAML metadata is available at \http://<host name>:9090/server/<database name>/<service name>/resources/metadata.xml

You will obtain something like this:

.. code-block:: xml
   :emphasize-lines: 4, 14

   <?xml version="1.0" encoding="UTF-8"?>
   <md:EntityDescriptor 
     xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata" 
     entityID="customer_report_ws_service_provider">
   
       <md:SPSSODescriptor AuthnRequestsSigned="false" 
         WantAssertionsSigned="true" 
         protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
           <md:NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified</md:NameIDFormat>
           <md:NameIDFormat>urn:oasis:names:tc:SAML:2.0:nameid-format:transient</md:NameIDFormat>
           <md:NameIDFormat>urn:oasis:names:tc:SAML:2.0:nameid-format:persistent</md:NameIDFormat>
           <md:AssertionConsumerService 
             Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" 
             Location="http://acme.denodo.com:9090/server/admin/customer_report_ws/" 
             index="0" 
             isDefault="true"/>
       </md:SPSSODescriptor>
   </md:EntityDescriptor>


"customer_report_ws_service_provider" is what you entered in the field *Service provider ID* of the configuration of the web service.

"Location" is what you entered in the field *Service provider URL base* of the dialog "SAML 2.0 configuration", in the global configuration of the Server (in this example, "\http://acme.denodo.com:9090"), followed by the relative URL of the service in the Denodo web container ("server/admin/customer_report_ws/").   

After deploying the web service, register the service in the identity provider (IdP). To do this, you need the service provider id (in the example above, ``customer_report_ws_service_provider``) and the URL to the metadata file (i.e. \http://<host name>:9090/server/<database name>/<service name>/resources/metadata.xml)

|

There are two ways of invoking web services with SAML authentication:

1. :ref:`From a browser<Invoking Web Services with SAML Authentication from a Browser>`
#. :ref:`From a client application <Invoking Web Services with SAML Authentication Programmatically>`


Invoking Web Services with SAML Authentication from a Browser
-------------------------------------------------------------

Follow these steps to invoke a REST web service that requires SAML authentication:

1. Open the browser and build a URL to send a request to the identity provider (IdP), not the REST service. This URL has to include the service you want to query.

   The way you invoke an identity provider changes from vendor to vendor. Let us say that in the vendor you are using, you have to indicate the request to the REST service in the URL parameter "target". In that case, you have to send a request like this:

   \https://identity_provider.acme.com/idp/profile/SAML2/Unsolicited/SSO?providerId=customer_report_ws_service_provider&target=views%2Fcustomer_report%3Fstate%3DCA

   The value of the parameter "target" is the relative URL (encoded) of the resource you want to query in the REST service (``views/customer_reports?state=CA``).

2. When the identity provider (IdP) receives this request, usually, it will show a form where the user has to enter her credentials. 

3. If the credentials are correct, the IdP will redirect the user's browser to a page of the IdP from which the user will be able to send a request to the REST service. 

4. The user sends a request to the REST service from this page. This is a POST request that includes a SAML assertion in the body.

5. The web service checks that the SAML assertion included in the request is valid. This indicates that the user is authorized to send requests to the service.

6. The web service obtains the user name from the SAML assertion - from the attribute "NameID" of the "Subject" of the assertion - and uses it to obtain the roles of this user so it knows what this user is authorized to do. The process of obtaining the roles is :ref:`the same as in LDAP databases <The LDAP Authentication Process>`.

Invoking Web Services with SAML Authentication Programmatically
---------------------------------------------------------------

To be able to send a request to a REST service that requires SAML authentication, the application has to obtain a SAML assertion first and then, send the request to the service. The application needs one assertion for each request because these REST services are stateless.

Usually, this application can use the API of the identity provider (IdP) to obtain this assertion.

The SAML assertions returned by the identity provider (IdP) have to meet these conditions, otherwise, the requests to the REST service will fail:

-  The assertions have to be signed.
-  The Subject of the assertions has to have the parameter "NameID". The service will use this value to obtain the roles of the user from the LDAP server you configured in the dialog “Server administration > SAML 2.0 configuration”.
-  The assertion that contains the bearer subject confirmation has to have the element ``<AudienceRestriction>`` with an element ``<Audience>`` whose value is the "Service provider ID" of the REST service.
     
The request sent to the service has to meet these conditions:

-  The request is a POST request to the root URL of the service, not the full URL.
-  The HTTP header ``Content-type`` has to be ``application/x-www-form-urlencoded``.
-  The body of the request has to have two fields:

   - ``RelayState``: relative URI to the resource you want to query.
   - ``SAMLResponse``: SAML assertion encoded in Base 64.

.. code-block:: none
   :caption: Sample HTTP request sent to a service with SAML authentication
   :emphasize-lines: 1,4

   POST /server/admin/customer_report_ws_service_provider/
   Content-Type: application/x-www-form-urlencoded

   RelayState=views%2Fcustomer%3Fyear%3D2016&SAMLResponse=PD94bWwgdmVyc2lvbj0iMS4wIiBlbm...
   
   
About this example:

-  The request is sent to the base URL of the service. As the service is called "customer_report_ws_service_provider" and is in the database "admin", the application sends the request to /server/admin/customer_report_ws_service_provider/

-  The body of this request has two attributes:

   1. RelayState: this is the relative URL of the resource you want to access and its parameters. In this case, ``views/customer?year=2016`` URL-encoded.
   #. SAMLResponse: the SAML assertion *encoded in Base64*.

A SAML assertion is similar to this:

.. code-block:: xml
   :name: Example SAML assertion non-encoded.

   <?xml version="1.0" encoding="UTF-8"?>
   <saml2p:Response xmlns:saml2p="urn:oasis:names:tc:SAML:2.0:protocol" xmlns:xs="http://www.w3.org/2001/XMLSchema" Destination="http://denodo-dv1-prod.denodo.com:9090/server/admin/internet_inc_ws/" ID="pfxb679929f-a8f8-6f04-1c86-54a6ac2439f0" IssueInstant="2015-03-31T20:08:33.405Z" Version="2.0">
       <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" Format="urn:oasis:names:tc:SAML:2.0:nameid-format:entity">IdentityProviderNameID</saml2:Issuer>
       <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
           .......................
       </ds:Signature>
       <saml2p:Status>
           <saml2p:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
       </saml2p:Status>
       <saml2:Assertion xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" ID="pfx23d93abd-1269-04f0-1c98-128d396a10d6" IssueInstant="2015-03-31T20:08:33.405Z" Version="2.0">
           <saml2:Issuer Format="urn:oasis:names:tc:SAML:2.0:nameid-format:entity">IdentityProviderNameID</saml2:Issuer>
           <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
               .......................
           </ds:Signature>
           <saml2:Subject>
               <saml2:NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified">userName</saml2:NameID>
               <saml2:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
                   <saml2:SubjectConfirmationData NotOnOrAfter="2015-03-31T20:13:33.405Z" Recipient="http://host:port/endpoint/url/"/>
               </saml2:SubjectConfirmation>
           </saml2:Subject>
           <saml2:Conditions NotBefore="2015-03-31T20:03:33.405Z" NotOnOrAfter="2015-03-31T20:13:33.405Z">
               <saml2:AudienceRestriction>
                   <saml2:Audience>internet_inc_service_provider</saml2:Audience>
               </saml2:AudienceRestriction>
           </saml2:Conditions>
           <saml2:AuthnStatement AuthnInstant="2015-03-31T20:08:33.405Z" SessionIndex="id1427832513405.1286465069">
               <saml2:AuthnContext>
                   <saml2:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport</saml2:AuthnContextClassRef>
               </saml2:AuthnContext>
           </saml2:AuthnStatement>
       </saml2:Assertion>
   </saml2p:Response>

     
Types Conversion Table for REST / SOAP Published Web Services
=============================================================

The table below lists the conversions from Virtual DataPort data types to the
SOAP and REST web service data types for the input and output parameters
of the web services.

 
.. table:: Conversions between Virtual DataPort data types and web service data types
   :name: Conversions between Virtual DataPort data types and web service data types

   +-------------------------+-------------------------+-------------------------+
   | Virtual DataPort Data   | SOAP Web Service Data   | REST Web Service Data   |
   | Type                    | Type                    | Type                    |
   +=========================+=========================+=========================+
   | blob                    | xsd:hexBinary           | xsd:hexBinary           |
   +-------------------------+-------------------------+-------------------------+
   | boolean                 | xsd:boolean             | xsd:boolean             |
   +-------------------------+-------------------------+-------------------------+
   | date                    | xsd:date or             | xsd:date or             |
   |                         | xsd:dateTime            | xsd:dateTime            |
   +-------------------------+-------------------------+-------------------------+
   | decimal                 | xsd:decimal             | xsd:decimal             |
   +-------------------------+-------------------------+-------------------------+
   | double                  | xsd:double              | xsd:double              |
   +-------------------------+-------------------------+-------------------------+
   | float                   | xsd:float               | xsd:float               |
   +-------------------------+-------------------------+-------------------------+
   | int                     | xsd:int                 | xsd:int                 |
   +-------------------------+-------------------------+-------------------------+
   | long                    | xsd:long                | xsd:long                |
   +-------------------------+-------------------------+-------------------------+
   | text                    | xsd:string              | xsd:string              |
   +-------------------------+-------------------------+-------------------------+
   | xml                     | xsd:string              | xsd:string              |
   +-------------------------+-------------------------+-------------------------+
   | array                   | soapenc:Array           | xsd:complexType         |
   |                         |                         | (sequence)              |
   +-------------------------+-------------------------+-------------------------+
   | register                | soapenc:complexType     | xsd:complexType         |
   |                         | (sequence)              | (sequence)              |
   +-------------------------+-------------------------+-------------------------+
