=====================================
Enabling SSL/TLS for External Clients
=====================================

JDBC and Other Java Clients
==============================================================

To secure the communication between Denodo servers and their JDBC
clients, set the Java system property
``javax.net.ssl.trustStore`` to point to the *TrustStore* that contains
the certificate used by the Denodo servers. For example:

-  For Windows:

.. code-block:: batch

   SET JAVA_OPTS=-Djavax.net.ssl.trustStore=<DENODO_HOME>/jre/lib/security/cacerts

-  For Unix:

.. code-block:: bash

   export JAVA_OPTS=-Djavax.net.ssl.trustStore=<DENODO_HOME>/jre/lib/security/cacerts

Some applications allow you to set this property without setting an
environment variable. For example, to launch JConsole, execute this:

.. code-block:: bash

   jconsole -J-Djavax.net.ssl.trustStore=<DENODO_HOME>/jre/lib/security/cacerts

In JConsole, when SSL is enabled, enter the URL of the Denodo server
with the format *<host>:<port>* instead of *<service:<…*.

ODBC Clients
==============================================================

The sections :doc:`Access through ODBC <../../../../vdp/developer/access_through_odbc/access_through_odbc>` and :doc:`Access through an Ado.Net Data
Provider <../../../../vdp/developer/access_through_an_ado.net_data_provider/access_through_an_ado.net_data_provider>` of the Virtual DataPort Developer Guide explain how to enable
the SSL communications in ODBC and Ado.Net clients respectively.
