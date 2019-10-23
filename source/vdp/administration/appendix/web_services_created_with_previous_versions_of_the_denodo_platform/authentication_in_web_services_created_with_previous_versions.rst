=============================================================
Authentication in Web Services Created with Previous Versions
=============================================================

.. warning:: This section is about the Web services created with previous
   version of the Denodo Platform.

The available authentication options depend on the service type:

For **REST**, **JSON**, **HTML** and **RSS** the options are:


-  **None**. The Service can be accessed without authentication.


-  **HTTP Basic**. `Basic HTTP authentication (RFC 2617: HTTP
   Authentication: Basic and Digest Access Authentication <https://www.ietf.org/rfc/rfc2617.txt>`_)
   with the credentials passed as
   plain text.

   All the users will use the values of the fields “Login” and “Password”
   as credentials for accessing this Service.


-  **HTTP Basic with VDP**. Basic HTTP authentication using the
   credentials of the Virtual DataPort users. That is, the Web service
   will connect to Virtual DataPort with the credentials used by the
   client of the Web service.

   Only users whose user name is in the “Accepted user(s)” list will have
   access to the Service (separate user names with commas). If this list
   is empty, the Web Service will accept all Virtual DataPort users.
   
   Unlike with the other authentication methods, with this one, you have
   to grant the user privileges to access the published views.

-  **HTTP Digest**. HTTP Digest access authentication.

The authentication options for **SOAP** Web Services are the same as for
REST, JSON, HTML and RSS, but also support the `Web Services Security
protocol (WS-Security) <https://www.oasis-open.org/committees/wss/>`_.
