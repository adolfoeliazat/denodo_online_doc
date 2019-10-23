=============================
Using Kerberos Authentication
=============================

To develop an application that logs into Virtual DataPort using Kerberos
authentication, do the following:

#. Add the parameter ``krbsrvname=HTTP`` to the connection string.
#. In the parameter ``Server`` of the connection string, put the full qualified domain 
   of the Virtual DataPort server. That is, what you have in the field *Server principal* of the Kerberos settings of the Virtual DataPort server, *without*
   the prefix *HTTP/* nor *@<domain name>*. I.e. if the server principal name, is
   ``HTTP/denodo-prod.subnet1.contoso.com@CONTOSO.COM``, the ``Server`` parameter has to be ``denodo-prod.subnet1.contoso.com``, not an alias you have on the DNS.

For example,

.. code-block:: java

   string connectionString =
        "Server=denodo1.subnet1.contoso.com;" +
        "Port=9996;" +
        "Database=admin" +
        "CommandTimeout=80000";

As we are using Kerberos authentication, we do not need to provide the
properties “Username” nor “Password” in the connection string.

To use Kerberos authentication, these conditions have to be met:

#. The application has to use the version 2.2.7 version of the Npgsql
   provider. Earlier versions do not support Kerberos authentication.
#. The Virtual DataPort database to which the application connects has
   to be configured with the option “ODBC/ADDO.net authentication type”
   set to “Kerberos”.
#. The host where the application runs has to belong to the Windows
   domain. The reason is that the adapter uses the ticket cache of the
   operating system to obtain “ticket-granting ticket” (TGT)
