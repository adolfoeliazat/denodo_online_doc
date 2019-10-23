===========================
Web Services Authentication
===========================

You can protect the access to a REST or SOAP Web service by configuring
the authentication method of the Service. The available authentication
methods depend on the Web service type:


.. table:: Authentication methods support by SOAP and REST Web services
   :name: Authentication methods support by SOAP and REST Web services table of the VQL Guide

   +----------------+----------------+----------------+----------------+----------------+
   | Authentication | Available in   | Available in   | Uses the       | Uses the       |
   |                | SOAP Web       | REST Web       | Credentials of | Credentials of |
   | Method         | Services       | Services       | the Web        | the Web        |
   |                |                |                | Service        | Service’s      |
   |                |                |                |                | Clients        |
   +================+================+================+================+================+
   | **HTTP Basic** | X              | X              | X              |                |
   +----------------+----------------+----------------+----------------+----------------+
   | **HTTP Basic   | X              | X              |                | X              |
   | with VDP**     |                |                |                |                |
   +----------------+----------------+----------------+----------------+----------------+
   | **HTTP         | X              | X              | X              |                |
   | Digest**       |                |                |                |                |
   +----------------+----------------+----------------+----------------+----------------+
   | **HTTP SPNEGO  | X              | X              |                | X              |
   | (Kerberos)**   |                |                |                |                |
   +----------------+----------------+----------------+----------------+----------------+
   | **OAuth 2.0**  | X              | X              |                | X              |
   +----------------+----------------+----------------+----------------+----------------+
   | **OpenID**     | X              | X              |                | X              |
   +----------------+----------------+----------------+----------------+----------------+
   | **SAML 2.0**   |                | X              |                | X              |
   +----------------+----------------+----------------+----------------+----------------+
   | **WSS Basic**  | X              |                | X              |                |
   +----------------+----------------+----------------+----------------+----------------+
   | **WSS Basic    | X              |                |                | X              |
   | with VDP**     |                |                |                |                |
   +----------------+----------------+----------------+----------------+----------------+
   | **WSS Digest** | X              |                | X              |                |
   +----------------+----------------+----------------+----------------+----------------+

When a Web Service uses the Virtual DataPort authentication methods
(``BASIC VDP`` and ``WSS VDP``), the clients of the Web service have to
use their Virtual DataPort credentials. That is, when a client sends a
request to one of these Services, the Service uses the credentials
provided by the client to open a connection to the Server and execute
the appropriate query. By setting this authentication method, the Server
can take into account the privileges of the user and its roles and her
custom policies.

This is not possible with the other authentication methods, because with
them, the Service uses the same connection with the Server to execute
all the queries.

The parameter ``VDPACCEPTEDUSERS`` of the ``BASIC VDP`` and ``WSS VDP``
is a comma-separated list of user names. Only users, whose user name is
in that list, will have access to the Service. If this parameter is
missing, the Service will accept all Virtual DataPort users.

Unlike with the other authentication methods, with this one, we have to
grant the user privileges to access the published views.

Basic and Digest
================

The ``BASIC`` and ``DIGEST`` authentication modes use the Basic and
Digest HTTP Access Authentication methods.

In HTTP Basic the credentials are passed as plaintext and in HTTP Digest
they are sent encrypted.

All the users will use the same credentials indicated in the parameters
``USER`` and ``PASSWORD``.

The ``ENCRYPTED`` modifier indicates that the password provided is
encrypted (this option is typically only used by the server
export/import metadata processes. Users do not need to use this option).

OAuth 2.0 and OpenID
=======================

To use these authentication methods on a web service, first you need to 
enable OAuth authentication on the Virtual DataPort server. The 
section :doc:`/vdp/administration/server_administration_-_configuring_the_server/oauth_authentication/oauth_authentication`
explains how to do this.

OpenID is an extension of OAuth 2.0. Denodo supports OpenID when it is configured to 
accept a JSON Web Tokens (JWT). That is, in the 
OAuth 2.0 configuration of the Server, the option *Use JWT* is selected.

SAML
====

The REST web services published by Virtual DataPort support SAML
authentication (Security Assertion Markup Language).

Before enabling SAML on a web service, you have to enable SAML on the
global configuration of the Server. The section “SAML Authentication”
explains how to do this. After doing this, you can publish web services
with this type of authentication.

Add the parameter ``SPENTITYID``, which is a string that identifies this
web service as a service provider with the identity provider (IdP).

The section :ref:`SAML 2.0` of the Administration Guide explains in more
detail how to configure web services with this authentication type.

VDP
===

When using the authentication methods ``BASIC VDP`` (SOAP and REST) and
``WSS BASIC VDP`` (only SOAP), the Web Service will connect to Virtual
DataPort with the credentials used by the client of the Web service.

Only users whose user name is in the ``VDPACCEPTEDUSERS`` list will have
access to the Service. If the list is empty, all Virtual DataPort
users will be accepted. With this authentication method, the users also
need to have permission to access the published views.

WSS
===

`Web Services Security (WSS) <https://www.oasis-open.org/committees/wss>`_
enforces integrity and confidentiality over
the Web service messaging. It works on top of the Basic or Digest
authentication methods. Currently, Virtual DataPort supports the
authentication profile called “Username Token”
