===================
Access Through ODBC
===================

.. toctree::
   :hidden:

   configuration_of_the_odbc_driver_on_windows/configuration_of_the_odbc_driver_on_windows.rst
   configuration_of_the_odbc_driver_in_linux_and_other_unix/configuration_of_the_odbc_driver_in_linux_and_other_unix.rst
   creating_a_dsn_less_connection/index.rst
   details_of_the_odbc_interface/details_of_the_odbc_interface.rst
   integration_with_third-party_applications/integration_with_third-party_applications.rst

ODBC (`Microsoft Open DataBase Connectivity <https://support.microsoft.com/kb/110093>`_) is a
standard API specification for using database management systems.

Denodo provides an ODBC driver for Windows and Linux and it also provides the source code to build the driver for other platforms. This driver is based on the ODBC PostgreSQL driver version 09.05.

With this ODBC driver, you can connect to Virtual DataPort
from applications that do not support JDBC, such as Excel.

.. note:: We recommend connecting to the Virtual DataPort server using the JDBC driver over the ODBC because the performance is better.

Backward Compatibility of the ODBC Driver
=========================================

The Virtual DataPort server is backward compatible with the ODBC driver and other clients, within the same major version. That is, the Denodo ODBC driver of an update can be used to connect to the Virtual DataPort server with the same update or a newer update. For example:

-  To connect to a Denodo server version 7.0 update 20181011, you can use the ODBC driver included in the same update or the driver included in 7.0 20180926, 7.0 20180611 or 7.0 GA.
-  Do not use the ODBC driver included in 7.0 20181011 to connect to a Denodo server version 7.0 update 20180926 because the driver is more recent than the server. This is an unsupported configuration; some operations could fail.
