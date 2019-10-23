===========================================
Connecting to Virtual DataPort with SSL/TLS
===========================================

If SSL was enabled in the Virtual DataPort server to secure the
communications, set the environment variable
``javax.net.ssl.trustStore`` to point to the trustStore that contains
the certificate used by the Denodo servers. Otherwise, the driver will
not be able to establish the connection with the Server.

See more about this in the section :doc:`Enabling SSL/TLS for External Clients </platform/installation/postinstallation_tasks/enable_ssl_connections_in_the_denodo_platform_servers/enabling_ssl_for_external_clients>`
of the Installation Guide.
