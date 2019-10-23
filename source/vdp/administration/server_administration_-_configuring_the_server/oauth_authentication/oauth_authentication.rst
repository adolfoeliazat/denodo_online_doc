.. _vdp_admin_guide_server_configuration_oauth_authentication:

=======================
OAuth Authentication
=======================

.. toctree::
   :hidden:

   setting-up_the_oauth_authentication_in_the_virtual_dataport_server.rst
   
Denodo can publish REST and SOAP web services that require authentication. One of the authentication protocols supported is OAuth 2.0.

An application that sends requests to an OAuth 2.0 service have to include an *access token* in the request. This access token has to be obtained by the application from the *authorization server*. The `standard OAuth 2.0 <https://tools.ietf.org/html/rfc6749>`_ does not define a protocol for the resource server (in this case, the Denodo server) to validate access tokens and to obtain meta-information from them. Several approaches have been developed by the community to bridge this gap. Denodo supports two of them:

-  `JSON Web Token (JWT) Profile for OAuth 2.0 Client Authentication and Authorization Grants <https://tools.ietf.org/html/rfc7523>`_. In this approach, the access tokens sent by applications contain a signature that must be validated in order to grant access to execute the request. After validating the token, it obtains the "scopes" defined in the token.
-  `OAuth 2.0 Token Introspection <https://tools.ietf.org/html/rfc7662>`_. With this method, the resource server (in this case, the Denodo server) sends the access token sent by the client application to an *introspection endpoint*. This endpoint returns a JSON document indicating if the access token is valid and meta information about it, including the "scopes" of the token. The Denodo server sends a request to this data source every time a client sends a request to a Denodo web service that is configured to use OAuth 2.0 authentication.

In both approaches, the goal is the same: 

1. To validate if the access token is valid and did not expire.
#. Obtain the names of the "scopes" contained in the token. The scopes of a token are, according to the standard, *A list of space-delimited, case-sensitive strings. The strings are defined by the authorization server.*
   
   In the case of Denodo, the scopes are names of roles. Once Denodo has the scopes of the token, it obtains the roles with the same names and executes the request with the privileges granted to these roles.

   Denodo ignores the scopes for which there is no role with the same name.
   
   If there is not any role with the same name as any of the scopes of the token, the request fails.
   
   
Creating the Roles of the Virtual DataPort Users
================================================

Before using OAuth authentication for web services, you have to either:

a. Create the appropriate roles so they match the names of the scopes included in the access tokens.

b. Or configure the identity manager of your organization so the scopes match the names of the roles you already defined in Denodo.

Remember that you need to grant the “Connect” privilege to the roles, in addition to the other privileges.

|

After doing this, go to the next section to learn how to configure the Denodo server.