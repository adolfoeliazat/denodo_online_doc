===========================
Web Services Authentication
===========================

You can protect the access to a REST or SOAP Web service by configuring
its authentication method. The available authentication methods are the
same for the REST and the SOAP Web services, but the SOAP ones also
support the `Web Services Security protocol (WS-Security) <https://www.oasis-open.org/committees/wss/>`_.

In addition to select an authentication method for the service, consider enabling SSL/TLS on
the Denodo web container to encrypt the data transferred by the web services. The section :ref:`Enabling HTTPS in the Embedded Apache Tomcat` of the Installation Guide explains how to do it.

.. table:: Authentication methods support by SOAP and REST Web services
   :name: Authentication methods support by SOAP and REST Web services
                                                                  
   +----------------+--------------+--------------+--------------+--------------+
   | Authentication | Available in | Available in | Uses the     | Uses the     |
   | Method         | SOAP Web     | REST Web     | Credentials  | Credentials  |
   |                | Services     | Services     | of the Web   | of the Web   |
   |                |              |              | Service      | Service’s    |
   |                |              |              |              | Clients      |
   +================+==============+==============+==============+==============+
   | **HTTP Basic** | X            | X            | X            |              |
   +----------------+--------------+--------------+--------------+--------------+
   | **HTTP Basic   | X            | X            |              | X            |
   | with VDP**     |              |              |              |              |
   +----------------+--------------+--------------+--------------+--------------+
   | **HTTP         | X            | X            | X            |              |
   | Digest**       |              |              |              |              |
   +----------------+--------------+--------------+--------------+--------------+
   | **HTTP SPNEGO  | X            | X            |              | X            |
   | (Kerberos)**   |              |              |              |              |
   +----------------+--------------+--------------+--------------+--------------+
   | **OAuth 2.0**  | X            | X            |              | X            |
   +----------------+--------------+--------------+--------------+--------------+
   | **SAML 2.0**   |              | X            |              | X            |
   +----------------+--------------+--------------+--------------+--------------+
   | **WSS Basic**  | X            |              | X            |              |
   +----------------+--------------+--------------+--------------+--------------+
   | **WSS Basic    | X            |              |              | X            |
   | with VDP**     |              |              |              |              |
   +----------------+--------------+--------------+--------------+--------------+
   | **WSS Digest** | X            |              | X            |              |
   +----------------+--------------+--------------+--------------+--------------+

HTTP Basic
============

**HTTP Basic** is basic HTTP authentication [HTTP-AUTH] with the credentials passed as plain text.
   
All the users will use the values of the fields “Login” and “Password”
as credentials for accessing this Service.
   
HTTP Basic with VDP
===========================

**HTTP Basic with VDP** is basic HTTP authentication using the
credentials of the Virtual DataPort users. That is, the Web service
will connect to Virtual DataPort with the credentials used by the
client of the Web service.

Only users whose user name is in the “Accepted user(s)” list will have
access to the Service (separate user names with commas). If this list
is empty, the Web Service will accept all Virtual DataPort users.

Unlike with the other authentication methods, with this one, you have
to grant the user privileges to access the published views.

HTTP Digest
========================

**HTTP Digest** access authentication. `HTTP Authentication Basic and Digest Access Authentication. <https://www.ietf.org/rfc/rfc2617.txt>`_


HTTP SPNEGO (Kerberos)
=======================

In order for the Kerberos-SPNEGO
authentication to work, the clients that send requests to the service
have to be logged in the same domain as the Virtual DataPort server.

This authentication method can only be used when the Kerberos
authentication is enabled on the Virtual DataPort Server. The section
:doc:`/vdp/administration/server_administration_-_configuring_the_server/kerberos_authentication/kerberos_authentication` explains how to enable this.

The section “Client Configuration” of `this article <https://www.oracle.com/technetwork/articles/idm/weblogic-sso-kerberos-1619890.html>`_ explains how to
configure Internet Explorer and Mozilla Firefox to use the
Kerberos-SPNEGO authentication. No special configuration is needed for Google Chrome.

When you connect from a browser to a service with this authentication, instead of having to enter your credentials, the browser will obtain a Kerberos ticket from the system and send it with the request. If the browser requests your credentials, it means that the browser is not correctly configured to use Kerberos authentication or that there was an authentication error.

To use Kerberos authentication from the browser, you do *not* need to modify the Windows registry as you have to do when using single sign-on from the administration tool.

OAuth 2.0
=======================

Before publishing a REST web service with OAuth 2.0 authentication, you have to set up the global OAuth settings of the Server on the “Server configuration” dialog. The section :doc:`/vdp/administration/server_administration_-_configuring_the_server/oauth_authentication/oauth_authentication` explains how to do it. Once you set up SAML globally, you can enable it on REST web services.

In REST web services with OAuth authentication, do not publish views whose data comes from data sources with *pass-through authentication*. The requests to these views will fail. That is because, when an application sends a request with OAuth authentication, it does not send a user/password. Instead, it sends an *OAuth access token*, from which is not possible to obtain a user and password to pass to the data source.

SAML 2.0
=====================

The REST web services published by Virtual DataPort support SAML 2.0 authentication (Security Assertion Markup Language); specifically, the “Web Browser SSO Profile” identity provider initiated with “HTTP POST Binding”.

The client applications need to obtain a new assertion for each request because the Denodo web services are stateless so they do not support sessions. 

.. important:: Before publishing a REST web service with SAML authentication, you have to set up the global SAML settings of the Server on the “Server configuration” dialog. The section :ref:`SAML AUTHENTICATION` explains how to do it. Once you set up SAML globally, you can enable it on REST web services.

At runtime, when a client sends a request to a REST web service with SAML authentication, the Server use the SAML protocol to authenticate the user. 

Then, it uses the LDAP settings of the dialog “Server configuration > SAML 2.0” to obtain the roles of the user. These roles determine if the user is allowed to perform the query.

When selecting the authentication method SAML 2.0, enter a value for **Service provider ID**. This is a string that identifies this web service as a service provider with the identity provider (IdP).

The section :ref:`Invoking Web Services with SAML Authentication` explains how to invoke a web service with SAML authentication from a browser and programmatically, from a client application.

In REST web services with SAML authentication, do not publish views whose data comes from data sources with *pass-through authentication*. The requests to these views will fail. That is because, when an application sends a request with OAuth authentication, it does not send a user/password. Instead, it sends a *SAML assertion*, from which is not possible to obtain a user and password to pass to the data source.